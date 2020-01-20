## v-if和v-for哪个优先级更高? 如果两个同时出现, 应该怎么优化得到更好的性能?

当`vue`处理命令时，`v-for` 比 `v-if` 具有更高的优先级，如下模板：

```
<ul>
  <li
    v-for="item in arrs"
    v-if="item.isActive"
    :key="item.id">
    {{ item.value }}
  </li>
</ul>
```

将会经过如下运算：

```
this.arrs.map(item =>{
	if(item.isActive){
		return item.value
	}
})
```

因此哪怕我们只需渲染出一小部分`arrs`的元素，也得每次渲染的时候遍历整个列表，不管`arrs`是否发生了变化。



优化方法：

1、将其更换为一个计算属性上的遍历，如下：

```
computed:{
	activeArrs(){
		return this.arrs.filter(item =>{
			return item.isActive
		})
	}
}
```

```
<ul>
	<li
		v-for="item in activeArrs"
		:key="item.id">
		{{ item.value }}
    </li>
</ul>
```

这样做我们会获得以下好处：

- 过滤后的列表只会在 `arrs` 数组发生相关变化时才被重新运算，过滤性更高效。

- 使用 `v-for="item in activeArrs"` 之后，我们在渲染的时候只遍历活跃元素，渲染更高效。

- 解耦渲染层的逻辑，可维护性更强

  

 2、为了获得同样的效果，我们也可以把： 

```
<ul>
	<li
		v-for="item in activeArrs"
		v-if="showActive"
		:key="item.id">
		{{ item.value }}
    </li>
</ul>
```

更改为：

```
<ul v-if="showActive">
	<li
		v-for="item in activeArrs"
		:key="item.id">
		{{ item.value }}
    </li>
</ul>
```

 通过将 `v-if` 移动到容器元素，我们不会再对列表中的*每个*用户检查 `showActive`。取而代之的是，我们只检查它一次，且不会在 `showActive`为否的时候运算 `v-for`。 