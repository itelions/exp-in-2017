## TreeList.vue

```html
<template>
    <div>
        <span v-if="label" @click="showValue(value)">{{label}}</span>
        <div class="group" v-if="list">
            <tree-list
                @select="showValue"
                :key="idx" 
                v-for="(item,idx) in list" 
                :list="item.children||null"
                :label="item.label"
                :value="item.value">
            </tree-list>
        </div>
    </div>
</template>

<script>
export default {
    name:'tree-list',
    props:[
        'list',
        'label',
        'value'
    ],
    methods:{
        showValue(value){
            this.$emit('select',value)
            // console.log(value)
        }
    }
}
</script>
```
