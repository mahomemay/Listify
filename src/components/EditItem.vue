<template>
    <div class="bg">
        <div class="box">
            <mu-text-field v-model="content" label="请输入新的待办内容" prefix=">" label-float></mu-text-field>
            <br/>
            <mu-switch v-model="important" :label="'是否标记为星标？'" label-left></mu-switch>
            <mu-flex justify-content="center" align-items="center">
                <mu-button round color="#555" @click="subEdit">确 定 修 改</mu-button>
            </mu-flex>
        </div>
    </div>
</template>

<script>
	export default {
		name: "EditItem",
        props: ['item', 'index'],
		data () {
			return {
				content: this.item.content,
                important: this.item.important,
				checked: false
			}
		},
        methods: {
			subEdit(){
				if(this.content !== '') {
					let newItem = {
						content: this.content,
                        important: this.important,
                        finished: this.item.finished
					}
					this.$emit('applyEdit', this.index, newItem)
                }
            }
        }
	}
</script>

<style scoped>
    .bg {
        background-color: #fff;
        border-radius: 10px;
        box-shadow: 0 5px 20px #00000012;
        padding: 10px 20px 20px;
        margin: 25px 20px;
    }
    .bg > .title {
        width: max-content;
        border-radius: 8px;
        background-color: #333;
        color: #fafafa;
        padding: 3px 15px;
        display: flex;
        align-items: center;
        transform: translate(5px, -15px);
    }
    .mu-input {
        width: 100%;
        min-height: auto;
        margin-bottom: 0;
        padding-bottom: 0;
    }
    .mu-input__focus {
        color: #666;
    }
    .mu-switch-checked {
        color: #666;
    }
    .checkbox {
        line-height: 30px;
        margin: 20px 0;
        display: flex;
        justify-content: space-between;
    }
    .mu-switch {
        display: flex;
        align-items: center;
    }
    .mu-input.has-icon {
        padding-left: 35px;
    }

</style>