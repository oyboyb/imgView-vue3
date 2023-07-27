<template>
    <div class="wrap" ref="wrapRef">
        <div class="item" v-for="({ name, category, createTime }, index) in imgArr" :key="name + index">
            <img :src="`/src/assets/${name}`" :alt="name" />
            <p>类别：{{ category }}</p>
            <p>时间：{{ createTime }}</p>
        </div>
    </div>
    <div class="btn-wrap">
        <select name="cars" @change="change">
            <option v-for="cate in category" :key="cate" :value="cate">{{ cate }}</option>
        </select>
        <button @click="btnClick">时间升序</button>
        <button @click="btnClick2">时间倒序</button>
    </div>
  </template>
  <script setup lang="ts">
  import { ref, onMounted } from 'vue'
  const imgArr = ref([
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
  const category = new Set(imgArr.value.map(({ category }) => category).concat(['全部']).reverse())
  let imgArrCopy:any = []
  imgArrCopy = [...imgArr.value]
  let selectType = ref('全部');
  const change = (e: any) => {
    selectType.value = e.target?.value
    imgArr.value = imgArrCopy
    if(selectType.value === '全部') return
    imgArr.value = imgArr.value.filter(f => f.category === selectType.value)
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
  
  
  const wrapRef = ref()
  onMounted(() => {
    //@ts-ignore
    new Sortable(wrapRef.value, {
        animation: 150,
        ghostClass: 'blue-background-class'
    });
  })
  </script>
  <style scoped>
  .wrap {
    width: 100%;
    height: 100%;
    margin: 0 auto;
    align-items: center;
  }
  .btn-wrap,.wrap{
    display: flex;
  }
  .btn-wrap select,button{
    width: 100px;
    height:30px;
    margin: 0 10px;
  }
  .wrap {
    flex-wrap: wrap;
    justify-content: center;
    border: 1px solid #aae;
    border-radius: 5px;
    margin-bottom: 10px;
    padding: 8px;
  }
  
  .item {
    width: 180px;
    height: 160px;
    margin: 8px;
  }
  
  img {
    display: block;
    width: 100%;
    height: 70%;
  }
  
  p {
    margin:5px 0 5px 0;
    font-size: 12px;
    color: #000;
  }
  </style>
  