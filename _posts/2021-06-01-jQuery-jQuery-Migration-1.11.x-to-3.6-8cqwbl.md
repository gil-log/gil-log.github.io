---
title: "[jQuery] jQuery Migration 1.11.x to 3.6 Deprecated FunctionList"
last_modified_at: 2021-06-01T23:24
categories: 
  - jquerymigration
tags: 
  - 'jquery' 
  - 'migration'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[jQuery Deprecated](https://api.jquery.com/category/deprecated/)

[jQuery Upgrade Guide](https://jquery.com/upgrade-guide/)

[jQuery Blog](https://jquery.com/upgrade-guide/)

[jQuery Migrate](https://github.com/jquery/jquery-migrate)

<br>

---

# jQuery Migration

회사에서 **jQuery를 사용**하고 있는데, **대표님께서 그놈의 jQuery를 다 걷어내고 `vanilla js` 로 전환 할지 고민**하시다, 결국 **코드 가독성과 편리성을 이유**로 **`Typescript` + `jQuery 3.6`**으로 **script단 기술 스택 전환**을 정하셨다.

**기존 System**에서는 **`jQuery 1.11.3` Version을 사용 중**이었는데, **`jQuery 3.6` 까지 Migration 과정**을 기록으로 남겨두려고 한다.
_혹시나 동일한 상황을 겪으실 다른 분들을 위해서나 나중에 또 써먹게 될 경우가 발생 했을 경우를 위해_


---


# 1. Deprecated Function List 확인

![](https://images.velog.io/images/gillog/post/6010ffc7-6b75-4e65-922e-f339ecd8ff09/image.png)

**jQuery 공식 Blog에 방문( _[jQuery Deprecated Function List](https://api.jquery.com/category/deprecated/)_ )**하면, **각 jQuery Version 별로 Deprecated 된 Function List를 확인**할 수 있다.


나는 **우선 각 jQuery Version 별로 Deprecated 되는 Function들을 확인 표로 정리** 하였다.



| Version |  Deprecated Function  | How To Upgrade |  Upgrade Iussue |
| :--: | :--: | :--: | :--: |
|   1.3   |    $.boxModel    |                     |                                                              |
|         |    $.browser     |          please rely on feature detection instead.           |          This  API has been removed in jQuery 1.9;           |
|   1.7   | deferred.isRejected() |             please use deferred.state() instead.             |          This  API has been removed in jQuery 1.8;           |
|         | deferred.isResolved() |            please  use deferred.state() instead.             |          This  API has been removed in jQuery 1.8;           |
|         |        .die()         |                                                              |                                                              |
|         |     $.sub()      |                                                              |          This  API has been removed in jQuery 1.9.           |
|         |        .live()        |                  please  use on() instead.                   |          This  API has been removed in jQuery 1.9;           |
|         |       .selector       |                                                              | Breaking  change: Deprecated <br>.context and .selector properties removed    <br> These properties were deprecated in jQuery 1.9, <br>as they were only used for  the obsolete <br>.live() method and have never accurately <br>represented the context <br> or selector for the current collection. |
|   1.8   |      .andSelf()       |                                                              |                                                              |
|         |    deferred.pipe()    | The deferred.then() method, which replaces it,  <br>should be used instead. | :As  of jQuery 1.8, the deferred.pipe() method is deprecated. |
|         |       .error()        | please use .on( "error", handler )  <br>instead of .error( handler ) <br>and .trigger( "error" ) instead of  .error(). |                                                              |
|         |        .load()        | please use .on( "load", handler ) <br> instead of .load( handler ) <br>and .trigger( "load" ) instead of  .load(). |           This API has been removed in jQuery 3.0;           |
|         |        .size()        |              Use the .length property instead.               |         This  method has been removed in jQuery 3.0          |
|         |       .toggle()       | jQuery also provides <br>an animation method  named .toggle() <br>that toggles the visibility of elements. <br>Whether the  animation or <br>the event method is fired <br>depends on the set of arguments  passed. | This  method signature was deprecated in jQuery 1.8 <br>and removed in jQuery 1.9 |
|         |       .unload()       |                                                              |                                                              |
|   1.9   |    $.support     | For your own project's feature-detection  needs, <br>we strongly recommend the use of <br>an external library such as Modernizr<br> instead of dependency <br>on properties in jQuery.support. | A  collection of properties<br> that represent the presence <br>of different browser  features or bugs.<br> Intended for jQuery's internal use;<br> specific properties may  be removed<br> when they are no longer needed <br>internally to improve page <br>startup  performance.<br>For your own project's <br>feature-detection needs,<br> we strongly  recommend the use of <br>an external library<br> such as Modernizr <br>instead of  dependency on properties<br> in jQuery.support. |
|  1.10.  |       .context        |                                                              | Breaking  change: Deprecated <br>.context and .selector properties removed  <br>   These properties were deprecated in jQuery 1.9,<br> as they were only <br>used for  the obsolete .live() method<br> and have never accurately <br>represented the context<br>  or selector for the current collection.   <br>  This API has been removed in jQuery 3.0. |
|  3.0.   |        .bind()        | For more flexible event  binding,<br> see the discussion of event delegation in .on(). | As  of jQuery 3.0, .bind() has been deprecated. <br>It was superseded by the .on()  method <br>for attaching event handlers <br>to a document since jQuery 1.7,<br> so its  use was already discouraged.<br>For earlier versions, <br>the .bind() method is used  for <br>attaching an event handler <br>directly to elements.<br> Handlers are attached to <br> the currently selected elements<br> in the jQuery object,<br> so those elements must  exist <br>at the point the call <br>to .bind() occurs.<br> For more flexible event  binding,<br> see the discussion of event delegation in .on(). |
|         |      .delegate()      | More information on event  binding <br>and delegation is in the .on() method.<br> In general, these are the  equivalent templates<br> for the two methods: | As  of jQuery 3.0, .delegate() has been deprecated. <br>It was superseded by the  .on() method<br> since jQuery 1.7, <br>so its use was already discouraged. <br>For  earlier versions, however,<br> it remains the most effective<br> means to use event  delegation. <br>More information on event binding <br>and delegation is in the .on()  method.<br> In general, these are the equivalent templates <br>for the two methods: |
|         |  $.fx.interval   |                                                              | Now  that requestAnimationFrame is being used<br> for animations,<br> the  jQuery.fx.interval property is ignored<br> on most browsers. <br>It is still present  in jQuery 3.0 <br>and used in browsers such as IE9, <br>but will be removed in a  future major-point release. |
|         |  $.parseJSON()   | To parse JSON strings <br>use  the native JSON.parse method instead. | Since  all the browsers supported by jQuery 3.0<br> support the native JSON.parse()  method,<br> we are deprecating jQuery.parseJSON(). |
|         |    $.unique()    | As of jQuery 3.0, this  method <br>is deprecated <br>and just an alias of jQuery.uniqueSort(). <br>Please use  that method instead. | The  jQuery.unique() method has been renamed to jQuery.uniqueSort() <br>to make its  behavior easier to understand. <br>There is no change to functionality here, only  a rename. |
|         |       .unbind()       | It was superseded by the .off() method <br>since  jQuery 1.7,<br>so its use was already discouraged. | Five  years ago in jQuery 1.7 <br>we introduced the .on() method <br> for attaching event  handlers. <br>The older .bind(), .unbind(), .delegate() <br>and .undelegate() methods  are being deprecated as of 3.0,<br> but are still present.<br> The API documentation  explains <br>how to rewrite the calls using <br>the .on() and .off() methods. |
|         |     .undelegate()     | It was superseded by the  .off() method <br>since jQuery 1.7,<br> so its use was already discouraged. | Five  years ago in jQuery 1.7 we introduced <br>the .on() method for attaching event  handlers.<br> The older .bind(), .unbind(), .delegate() and .undelegate() methods  are being deprecated as of 3.0,<br> but are still present.<br> The API documentation  explains<br> how to rewrite the calls using the .on() and .off() methods. |
|   3.2   |  $.holdReady()   |                                                              |                                                              |
|         |   $.isArray()    |     please use the native Array.isArray method  instead.     |                                                              |
|   3.3   |  $.isFunction()  | In most cases, <br>its use  can be replaced <br>by typeof x === "function". |                                                              |
|         |  $.isNumeric()   |                                                              |         This  API has been deprecated in jQuery 3.3.         |
|         |   $.isWindow()   | if you need this  function,<br> reimplement it by yourself:    <br>    function isWindow( obj ) {    <br>  return obj != null && obj  === obj.window;<br>   } <br>|         This  API has been deprecated in jQuery 3.3;         |
|         |     $.now()      |      please use the native <br> Date.now() method instead.       | This API has been deprecated in jQuery  3.3;   <br>  The $.now() method is an alias for Date.now(). |
|         |    $.proxy()     | please use the native  <br>Function.prototype.bind method instead. |                                                              |
|         |     $.type()     |                                                              |         This  API has been deprecated in jQuery 3.3.         |
|   3.4   |    :eq()<br> Selector     | Remove it from your  selectors <br>and filter the results later using .eq(). |                                                              |
|         |    :even<br> Selector     | . Remove it from your  selectors <br>and filter <br>the results later using .even()<br> (available in jQuery  3.5.0 or newer). |                                                              |
|         |    :first<br> Selector    | Remove it from your selectors <br>and filter the  results later using .first(). |                                                              |
|         |    :gt()<br> Selector     | For example, :gt(3) can be replaced <br>with a  call to .slice( 4 )<br> (the provided index needs<br> to be increased by one). |                                                              |
|         |    :last <br>Selector     | .  Remove it from your selectors<br> and filter the results <br>later using .last(). |                                                              |
|         |    :lt()<br> Selector     | Remove it from your  selectors <br>and filter the results<br> later using .slice().<br> For example, :lt(3)  can be replaced with <br>a call to .slice( 0, 3 ). |                                                              |
|         |     :odd<br> Selector     |                                                              |                                                              |
|   3.5   |     jQuery.trim()     | ; please use the native  String.prototype.trim method instead. |                                                              |

위에서 **정리한 Deprecated Function List**를 가지고,** 
우리 회사 System에서 `Ctrl` + `H` 검색**으로 **사용 중인 Libary 포함, 
Depreacted Function이 사용 중인지 우선 간략적으로 확인**하였다.

![](https://images.velog.io/images/gillog/post/ca6521d1-6c31-4277-b4f9-5a7a07fe9bd5/image.png)



