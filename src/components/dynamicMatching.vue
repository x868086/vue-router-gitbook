<template>
  <section class="normal markdown-section">

    <h1 id="动态路由匹配">动态路由匹配</h1>
    <p>我们经常需要把某种模式匹配到的所有路由，全都映射到同个组件。例如，我们有一个
      <code>User</code> 组件，对于所有 ID 各不相同的用户，都要使用这个组件来渲染。那么，我们可以在
      <code>vue-router</code> 的路由路径中使用『动态路径参数』（dynamic segment）来达到这个效果：</p>
    <pre><code class="lang-js"><span class="hljs-keyword">const</span> User = {
      template: <span class="hljs-string">'&lt;div&gt;User&lt;/div&gt;'</span>
    }

    <span class="hljs-keyword">const</span> router = <span class="hljs-keyword">new</span> VueRouter({
      routes: [
        <span class="hljs-comment">// 动态路径参数 以冒号开头</span>
        { path: <span class="hljs-string">'/user/:id'</span>, component: User }
      ]
    })
    </code></pre>
    <p>现在呢，像
      <code>/user/foo</code> 和
      <code>/user/bar</code> 都将映射到相同的路由。</p>
    <p>一个『路径参数』使用冒号
      <code>:</code> 标记。当匹配到一个路由时，参数值会被设置到
      <code>this.$route.params</code>，可以在每个组件内使用。于是，我们可以更新
      <code>User</code> 的模板，输出当前用户的 ID：</p>
    <pre><code class="lang-js"><span class="hljs-keyword">const</span> User = {
      template: <span class="hljs-string">'&lt;div&gt;User { $route.params.id }}&lt;/div&gt;'</span>
    }
    </code></pre>
    <p>你可以看看这个
      <a href="http://jsfiddle.net/yyx990803/4xfa2f19/" target="_blank">在线例子</a>.</p>
    <p>你可以在一个路由中设置多段『路径参数』，对应的值都会设置到
      <code>$route.params</code> 中。例如：</p>
    <table>
      <thead>
        <tr>
          <th>模式</th>
          <th>匹配路径</th>
          <th>$route.params</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>/user/:username</td>
          <td>/user/evan</td>
          <td>
            <code>{ username: 'evan' }</code>
          </td>
        </tr>
        <tr>
          <td>/user/:username/post/:post_id</td>
          <td>/user/evan/post/123</td>
          <td>
            <code>{ username: 'evan', post_id: 123 }</code>
          </td>
        </tr>
      </tbody>
    </table>
    <p>除了
      <code>$route.params</code> 外，
      <code>$route</code> 对象还提供了其它有用的信息，例如，
      <code>$route.query</code>（如果 URL 中有查询参数）、
      <code>$route.hash</code> 等等。你可以查看
      <a href="../api/route-object.html">API 文档</a> 的详细说明。</p>
    <h3 id="响应路由参数的变化">响应路由参数的变化</h3>
    <p>提醒一下，当使用路由参数时，例如从
      <code>/user/foo</code> 导航到
      <code>user/bar</code>，
      <strong>原来的组件实例会被复用</strong>。因为两个路由都渲染同个组件，比起销毁再创建，复用则显得更加高效。
      <strong>不过，这也意味着组件的生命周期钩子不会再被调用</strong>。</p>
    <p>复用组件时，想对路由参数的变化作出响应的话，你可以简单地 watch（监测变化）
      <code>$route</code> 对象：</p>
    <pre><code class="lang-js"><span class="hljs-keyword">const</span> User = {
      template: <span class="hljs-string">'...'</span>,
      watch: {
        <span class="hljs-string">'$route'</span> (to, <span class="hljs-keyword">from</span>) {
          <span class="hljs-comment">// 对路由变化作出响应...</span>
        }
      }
    }
    </code></pre>
    <h3 id="高级匹配模式">高级匹配模式</h3>
    <p>
      <code>vue-router</code> 使用
      <a href="https://github.com/pillarjs/path-to-regexp" target="_blank">path-to-regexp</a> 作为路径匹配引擎，所以支持很多高级的匹配模式，例如：可选的动态路径参数、匹配零个或多个、一个或多个，甚至是自定义正则匹配。查看它的
      <a href="https://github.com/pillarjs/path-to-regexp#parameters" target="_blank">文档</a> 学习高阶的路径匹配，还有
      <a href="https://github.com/vuejs/vue-router/blob/next/examples/route-matching/app.js" target="_blank">这个例子 </a> 展示
      <code>vue-router</code> 怎么使用这类匹配。</p>
    <h3 id="匹配优先级">匹配优先级</h3>
    <p>有时候，同一个路径可以匹配多个路由，此时，匹配的优先级就按照路由的定义顺序：谁先定义的，谁的优先级就最高。</p>

  </section>
</template>
