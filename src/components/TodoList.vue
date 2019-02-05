<template>
    <div class="bg">
        <div class="title">To do
            <mu-badge :content="items.length.toString()" circle color="#999"  class="demo-icon-badge"></mu-badge>
        </div>
        <ul>
            <li v-for="(item, i) in items">
                <mu-expansion-panel :expand="panel === 'panel1'" @change="toggle('panel')">
                    <div slot="header">
                        <mu-checkbox :class="{finished: item.finished, important: item.important}" :value="'Checkbox ' + i" v-model="item.finished" :label="item.content"></mu-checkbox>
                    </div>
                    <mu-tooltip content="编辑此条">
                        <mu-button small fab color="#555" @click="$emit('editItem', i)">
                            <mu-icon value="edit"></mu-icon>
                        </mu-button>
                    </mu-tooltip>
                    <mu-tooltip content="删除此条">
                        <mu-button small fab color="#555" @click="$emit('delItem', i)">
                            <mu-icon value="delete_sweep"></mu-icon>
                        </mu-button>
                    </mu-tooltip>
                    <mu-tooltip content="加入星标">
                        <mu-button small fab color="#555" @click="$emit('impItem', i)">
                            <mu-icon value="star"></mu-icon>
                        </mu-button>
                    </mu-tooltip>
                </mu-expansion-panel>
            </li>
        </ul>
    </div>
</template>

<script>
	export default {
		name: "TodoList",
		data(){
			return {
				panel: ''
			}
		},
        methods: {
			toggle (panel) {
				this.panel = panel === this.panel ? '' : panel;
			},
			itemId(id, num){
				switch (num) {
                    case 0:
                    	this.panel = ''
						this.$emit('editItem', id)
						break
                    case 1:
						this.$emit('delItem', id)
                        break
                    case 2:
						this.$emit('impItem', id)
                        break
                }
            }
        },
        props: ['items'],
	}
</script>

<style scoped>
    .bg {
        background-color: #fff;
        border-radius: 10px;
        box-shadow: 0 5px 20px #00000012;
        padding: 0 10px 20px;
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
    /* MuseUI Style Rewrite */
    .mu-badge-container {
        transform: translate(5px, -1px);
    }
    .mu-elevation-2 {
        box-shadow: none;
    }
    .mu-checkbox-checked {
        color: #333;
    }
    .mu-button {
        margin: 0 10px;
    }
    .finished {
        color: #999;
        text-decoration: line-through;
    }
    .important {
        font-size: 18px;
        font-weight: 500;
    }
</style>
