### 封装Dialog组件

```html
<!-- Dialog.vue -->
<template>
    <div class="bg-mask" @touchmove="disableScroll" >
        <section class="dialog">
            <div class="title">{{title}}</div>
            <div class="content">{{content}}</div>
            <div class="btn-group">
                <buttom @click="okCb">确定</buttom>
                <buttom @click="cencelCb">取消</buttom>
            </div>
        <section>
    </div>
</template>
<script>
    export default {
        data () {
            return {
                show: false,
                cencelable: false,
                title: '温馨提示',
                content: '这是个对话框'
            }
        },
        methods： {
            success: function() {},
            cencel: function() {},
            okCb: function() {
                this.show = !this.show
                if (typeof this.success === 'function') {
                    this.success()
                }
            },
            cencelCb: function() {
                this.show = !this.show
                if (typeof this.cencel === 'function') {
                    this.cencel()
                }
            },
            disableScroll ($event) {
                $event.preventDefault()
            }
        }
    }
</script>
```
```javascript
const = DialogComponent = require('./Dialog')
let $vm

const plugin = {
    install(Vue) {
        if (!$vm) {
            const Dialog = Vue.extend(DialogComponent)
            $vm = new Dialog({
                el: document.createElement('div')
            })
            document.body.appendChild($vm.$el)
        }
        const defaultOpt = {
            show: false,
            title: '',
            content: '内容',
            success: function() {},
            cancel: function() {}
        }
        const dialog = {
            show (opt) {
                for (let key in defaultOpt) {
                    // 如果show没配置参数，将用defaultOpt的
                    $vm[key] = opt[key] || defaultOpt[key]
                }
                $vm.show = true
            },
            hide () {
                $vm.show = false
            }
        }

        if (!Vue.$ui) {
            Vue.$ui = {
                dialog
            }
        } else {
            Vue.$ui.dialog = dialog
        }
        // 全局混入，也可挂载到Vue.prototype上面
        Vue.mixin({
            created: function() {
                this.$ui = Vue.$ui
            }
        })
    }
}
export default plugin
```

使用： 
```javascript
this.$ui.dialog({
    title: '这是我的标题',
    content: '这是我的内容',
})
```