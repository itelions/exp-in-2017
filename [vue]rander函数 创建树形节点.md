# vue rander 函数 创建树形组件
[vue rander 函数](https://cn.vuejs.org/v2/guide/render-function.html)
```js
'tree':{
   name:'tree',
   render(c){
        if(this.list&&this.list.length){
            return c('ul',this.list.map((item)=>{
                var props;
                if(item.children){
                    return c('li',[
                        c('div',{
                             on:{
                                'click':()=>{
                                   this.$emit('node-click',item.value)
                                }
                            }
                        },
                            item.label
                        ),
                        c('tree',{
                            props:{list:item.children},
                            on:{
                                'node-click':(value)=>{
                                    this.$emit('node-click',value);
                                }
                            }
                        })
                    ])
                }else{
                    return c('tree',{
                        props:{
                        label:item.label, 
                        value:item.value
                        },
                        on:{
                            'node-click':(value)=>{
                                this.$emit('node-click',value);
                            }
                        }
                    })
                }
            }))
        }else{
            return c('li',{
                on:{
                    'click':()=>{
                        this.$emit('node-click',this.value);
                    }
                }
            },this.label)
        }
   },
   props:[
       'list',
       'label',
       'value'
   ]
}
```