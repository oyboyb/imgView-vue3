<!-- eslint-disable vue/valid-v-else -->
<template>
    <div class="wrap" :class="[selectType =='全部' ? 'center':'']" ref="wrapRef">
        <div class="category" ref="" v-if="selectType!=='全部'" v-for="(cateData, category) in imgArr" :data-category="category" :key="category">
            <div class="title">分类：{{category }}</div>
            <!-- @vue-ignore -->
            <div class="item" v-for="({ name, category, createTime, id}, index) in cateData" :data-id="id" :key="id">
                <img :src="`/src/assets/${name}`" :alt="name" />
                <p>类别：{{ category }}</p>
                <p>时间：{{ createTime }}</p>
            </div>
        </div>
        <div class="item" :class="[selectType =='全部' ? 'item-margin':'']" v-else v-for="({ name, category, createTime }, index) in imgArr" :key="name + index">
                <img :src="`/src/assets/${name}`" :alt="name" />
                <p>类别：{{ category }}</p>
                <p>时间：{{ createTime }}</p>
        </div>
    </div>
    <div class="btn-wrap">
        <span>分类：</span>
        <select name="cars" @change="change2">
            <option v-for="cate in ['全部','分类']" :key="cate" :value="cate">{{ cate }}</option>
        </select>
        <button @click="btnClick" :disabled="selectType !=='全部'">时间升序</button>
        <button @click="btnClick2" :disabled="selectType !=='全部'">时间倒序</button>
    </div>
</template>
<script setup lang="ts">
import { ref, onMounted,nextTick} from 'vue'
let imgArr = ref([
    {
        createTime: '2023-7-25-21:05:46',
        category: '表情',
        name: '表情包 (1).gif',
    },
    {
        createTime: '2023-7-24-21:05:46',
        category: '表情',
        name: '表情包 (1).jpg'
    },
    {
        createTime: '2023-7-23-21:05:46',
        category: '产品',
        name: '产品.png'
    },
    {
        createTime: '2023-7-22-21:05:46',
        category: '表情',
        name: '表情包 (2).jpg'
    },
    {
        createTime: '2023-7-21-21:05:46',
        category: '表情',
        name: '表情包 (3).jpg'
    },
    {
        createTime: '2023-7-20-21:05:46',
        category: 'logo',
        name: 'vite.svg'
    },
    {
        createTime: '2023-7-19-21:05:46',
        category: 'logo',
        name: 'logo.svg'
    },
    {
        createTime: '2023-7-18-21:05:46',
        category: 'logo',
        name: 'vite.svg'
    },
    {
        createTime: '2023-7-17-21:05:46',
        category: 'logo',
        name: 'logo.svg'
    }, {
        createTime: '2023-7-16-21:05:46',
        category: 'logo',
        name: 'vite.svg'
    },
    {
        createTime: '2023-7-15-21:05:46',
        category: '风景',
        name: '海滩.jpg'
    },
    {
        createTime: '2023-7-15-21:05:46',
        category: '风景',
        name: '海滩.jpg'
    },
    {
        createTime: '2023-7-15-21:05:46',
        category: '风景',
        name: '海滩.jpg'
    },
    {
        createTime: '2023-7-15-21:05:46',
        category: '风景',
        name: '海滩.jpg'
    },
    {
        createTime: '2023-7-15-21:05:46',
        category: '风景',
        name: '海滩.jpg'
    },
    {
        createTime: '2023-7-14-21:05:46',
        category: '人物',
        name: '恶搞.jpg'
    },
    {
        createTime: '2023-7-14-21:05:46',
        category: '人物',
        name: '恶搞.jpg'
    },
    {
        createTime: '2023-7-14-21:05:46',
        category: '人物',
        name: '恶搞.jpg'
    },
    {
        createTime: '2023-7-14-21:05:46',
        category: '人物',
        name: '恶搞.jpg'
    },
    {
        createTime: '2023-7-14-21:05:46',
        category: '人物',
        name: '恶搞.jpg'
    }
])
new Date().getTime() + Math.random().toString(36).substr(2);
// @ts-ignore
imgArr.value.forEach(arr => arr['id'] = new Date().getTime() + Math.random().toString(36).substr(2))
const sortArr = () => {
    return Math.random() > 0.5 ? -1 : 1;
}

imgArr.value = imgArr.value.sort(sortArr)
// const category = new Set(imgArr.value.map(({ category }) => category).concat(['全部']).reverse())

let imgArrCopy: any = []
imgArrCopy = [...imgArr.value]
let selectType = ref('全部');
const wrapRef = ref()
const change2 = (e: any) => {
    selectType.value = e.target.value
    imgArr.value = [...imgArrCopy]
    if(selectType.value === '全部') return imgArr.value
    imgArr.value = imgArr.value.reduce((a,b)=>{
        const { category } = b;
        a[category] = a[category] ?? [];
        a[category].push(b);
        return a;
    },{})
    nextTick(()=>{
        console.log(wrapRef.value.children.length);
        Array.from(wrapRef.value.children).forEach((child)=>{
            //@ts-ignore
            new Sortable(child, {
                animation: 150,
                group: 'shared',
                filter:'.title',
                draggable: ".item",
                ghostClass: 'blue-background-class',
                onAdd:function (evt:any) {
                    const { category:oldCategory } = evt.from.dataset
                    const { category } = evt.to.dataset
                    const { id } = evt.item.dataset
                    // console.log(evt,oldCategory)
                    let cur = (imgArr.value[oldCategory] as any).find(arr =>arr.id === id)
                    if(cur) {
                        cur.category = category 
                    } else {
                        // TODO之前有偶现的报错，但目前没复现到
                        console.log(imgArr.value,oldCategory,id);
                    }
                } 
            });
        })
    })
}
// 时间升序 
const btnClick = () => {
    imgArr.value.sort((a, b) => {
        return new Date(a.createTime).getTime() > new Date(b.createTime).getTime() ? 1 : -1
    })
}
// 时间倒序
const btnClick2 = () => {
    imgArr.value.sort((a, b) => {
        return new Date(a.createTime).getTime() < new Date(b.createTime).getTime() ? 1 : -1
    })
}

</script>
<style scoped>
.wrap {
    width: 100%;
    height: 100%;
    margin: 0 auto;
    align-items: center;
}
.category,
.btn-wrap,
.wrap {
    display: flex;
}
.wrap,
.category {
    border: 1px solid #aae;
    border-radius: 5px;
    margin-bottom: 10px;
    padding: 8px;
}
.category {
    min-width: 100%;
    min-height: 178px;
}
.btn-wrap select,
button {
    width: 100px;
    height: 30px;
    margin: 0 10px;
}
.center{
    justify-content: center;
}
.wrap {
    flex-wrap: wrap;
}

.item {
    width: 180px;
    height: 160px;
    margin: 0 8px;
    box-shadow: rgba(60, 64, 67, 0.3) 0px 1px 2px 0px, rgba(60, 64, 67, 0.15) 0px 2px 6px 2px;
}
.item-margin:not(:last-child) {
    margin-bottom: 8px;
}
img {
    display: block;
    width: 100%;
    height: 70%;
}

p { 
    padding-left: 4px;
    padding-top: 4px;
    font-size: 12px;
    color: #000;
}</style>
