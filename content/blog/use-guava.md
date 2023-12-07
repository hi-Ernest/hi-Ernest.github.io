---
title: 使用Guava优雅解决代码边界判断
date: 2023-12-06T19:17:08+08:00
tag: ["旅行"]
---
### 背景

在Java编程中需要对数据库操作、RPC请求等返回的数据进行空值判断，以避免空指针错误和数据越界等问题。
此外需要在边界情况下提前进行断言并包装友好的错误信息，以便向用户提供清晰的报错信息。

**由于容易忽略边界情况的空值判断，每一步都需要考虑这些情况，导致模板代码过多，缺乏直观和统一的风格**

### 现状code

```java
  public SceneInfo getSceneInfo(GetSceneInfoRequest request) {
    // getPlanId 在调用方法入口 强制判空了
    String planId = request.getPlanId();
    TPlanEntity entity = tPlanMapper.getPlanInfo(planId);
    logger.info("预案信息 planEntity:{}", JsonUtils.toJsonString(planEntity));
    // 判断是否为null
    if (entity == null) {
      throw new BizException("预案不存在");
    }
    SceneInfo info = new SceneInfo();
    String sceneName = planEntity.getSceneName();
    if (StringUtils.isNotBlank(sceneName)) {
      List<SceneDto> sceneDtoList = RedHaClient.sceneDtoMap.get(sceneName);
      // 判断是否list是否为空
      if (sceneDtoList == null || sceneDtoList.isEmpty()) {
        throw new BizException("预案绑定的业务场景不存在");
      }
      sceneDtoList.stream()
          .findFirst()
          .ifPresent(
              dto -> {
                info.setName(dto.getName());
                info.setNameZh(dto.getNameZh());
              });
    }
    // 校验预案是否绑定的业务场景id
    Integer id = planEntity.getSceneId();
    // 继续判断
    if (id == null || id  == 0) {
      throw new BizException("该预案没有绑定业务场景");
    }
    // 查询预案绑定的业务场景信息
    SceneEntity entity = sceneService.getSceneEntity(id);
    String sceneZhName = entity.getNameZh();
    String sceneZhName = entity.getNameZh();
    // 继续判断
    if (sceneZhName == null || sceneZhName.equals("")) {
      throw new BizException("该预案绑定的业务场景信息为空");
    }
    //xxxx其他操作
    return info;
}
```



### 优雅处理边界byGuava

#### 什么是Guava

Guava是由Google开发的Java集合库，旨在提供处理Java集合的实用工具。它包括许多功能，如基本工具、集合、不可变集合、新的集合类型、图形、缓存、功能习语、并发、字符串、原始类型、范围等。Guava的目标是减少编码错误，促进标准编码实践，并通过使代码简洁易读来提高生产效率；总的来说，Guava是一个功能强大的Java集合库，旨在简化Java编程，并提供各种实用工具和功能，以提高开发效率和减少错误。
> https://github.com/google/guava


### show code （避免遗漏空值判断、精简代码减少模版代码）

```java
  public SceneInfo getSceneInfo(GetSceneInfoRequest request) {
    String planId = request.getPlanId();
    // 查询预案信息
  // 链式方式获取数据时候，顺便判断边界条件 + 不符合条件的报错信息
    TPlanEntity planEntity =
        Preconditions.checkNotNull(tPlanMapper.getPlanInfo(planId), "预案不存在 planId: %s", planId);
    logger.info("预案信息 planEntity:{}", JsonUtils.toJsonString(planEntity));
    // 新预案有记录ha业务场景name直接返回
    SceneInfo info = new SceneInfo();
    String sceneName = planEntity.getSceneName();
    if (StringUtils.isNotBlank(sceneName)) {
// 链式方式获取数据时候，顺便判断边界条件 + 不符合条件的报错信息
      List<SceneDto> sceneDtoList =
          Preconditions.checkNotNull(
              RedHaClient.sceneDtoMap.get(sceneName), "该预案绑定的业务场景英文映射在HA平台不一致");
      // 这里获取sceneDtoList 保证百分之百不为空
      sceneDtoList.stream()
          .findFirst()
          .ifPresent(
              dto -> {
                info.setName(dto.getName());
                info.setNameZh(dto.getNameZh());
              });
      return info;
    }
    // 校验预案是否绑定的业务场景id
    // 使用Optional包装返回数据，强制下一步判断边界，现有api名称见面直意
    Optional<Integer> sceneIdOptional = Optional.ofNullable(planEntity.getSceneId());
    if (!sceneIdOptional.isPresent() || sceneIdOptional.get() == 0) {
      throw new BizException("该预案没有绑定业务场景");
    }
    // 查询预案绑定的业务场景信息
    SceneEntity entity = sceneService.getSceneEntity(sceneIdOptional.get());
    String sceneZhName = entity.getNameZh();
    if (Strings.isNullOrEmpty(sceneZhName)) {
      throw new BizException("该预案绑定的业务场景信息为空");
    }
// 或者这个case  
    // 期望数据为true，不满足则报错
    // Preconditions.checkArgument(!Strings.isNullOrEmpty(sceneZhName), "预案绑定的业务场景信息为空");
    // xxxx其他操作
    return info;
}
```

### Quick Start

```xml
        <dependency>
            <groupId>com.google.guava</groupId>
            <artifactId>guava</artifactId>
            <version>32.1.3-jre</version>
        </dependency>
```