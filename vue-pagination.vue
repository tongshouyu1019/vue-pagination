<template>
  <div class="{{wrapperClass}}">
    <a class="button stat">
      {{currentPage}}/{{lastPage}}
    </a>
    <!-- 向前翻页 -->
    <a v-link="getUrl(1)" v-if="currentPage > 2"
      class="button">
      &laquo;
    </a>
    <a v-link="getUrl(currentPage-1)" v-if="currentPage > 1"
      class="button">
      &nbsp;&lsaquo;
    </a>
    <!-- 分页主体 -->
    <template v-for="n in pageShow">
      <a v-link="getUrl(n)"
        class="button">
        {{ n }}
      </a>
    </template>
    <!-- 向后翻页 -->
    <a v-link="getUrl(currentPage+1)" v-if="currentPage < lastPage"
      class="button">
      &rsaquo;&nbsp;
    </a>
    <a v-link="getUrl(lastPage)" v-if="currentPage < lastPage-1"
      class="button">
      &raquo;
    </a>
  </div>
</template>

<script>
  export default {
    props: {
      'wrapperClass': {
        type: String,
        default() {
          return 'pagination';
        },
      },
      'onEachSide': {
        type: Number,
        // 在设置值之前转换值
        coerce(value) {
          return parseInt(value);
        },
        default() {
          return 2;
        },
      },
      'currentPage': {
        type: Number,
        coerce(value) {
          return parseInt(value);
        },
        default() {
          return 0;
        },
      },
      'lastPage': {
        type: Number,
        coerce(value) {
          return parseInt(value);
        },
        default() {
          return 0;
        },
      },
      'rule': {
        type: String,
        default() {
          return '*';
        }
      }
    },
    data() {
      return {
        pageShow: [],
      }
    },
    computed: {
      windosize() {
        return this.onEachSide * 2 + 1;
      },
    },
    ready() {
      const showArr = this.pageShow,
            last = this.lastPage,
            current = this.currentPage,
            size = this.windosize,
            eachSide = this.onEachSide;

      if(last <= size) {
        this.pushArr(1, last, showArr);

      }else if(current <= eachSide) {
        this.pushArr(1, size, showArr);

      }else if(current >= last - eachSide) {
        const s = last - eachSide * 2;
        this.pushArr(s, last, showArr);

      }else{
        const s = current - eachSide,
              e = current + eachSide;
        this.pushArr(s, e, showArr);
        
      }
    },
    methods: {
      pushArr(s,e,arr) {
        for(let i = s; i <= e; i++) {
          arr.push(i);
        }
      },
      getUrl(n) {
        return this.rule.replace(/\*/, n); 
      },
    },
  }
</script>

<style scoped>   
  .button {
    -moz-appearance: none;
    -webkit-appearance: none;
    -webkit-box-align: center;
    -webkit-align-items: center;
        -ms-flex-align: center;
            align-items: center;
    background-color: white;
    border: 1px solid #d3d6db;
    border-radius: 3px;
    color: #222324;
    display: -webkit-inline-box;
    display: -webkit-inline-flex;
    display: -ms-inline-flexbox;
    display: inline-flex;
    font-size: 14px;
    height: 32px;
    -webkit-box-pack: start;
    -webkit-justify-content: flex-start;
        -ms-flex-pack: start;
            justify-content: flex-start;
    line-height: 24px;
    padding-left: 8px;
    padding-right: 8px;
    position: relative;
    vertical-align: top;
    -webkit-box-pack: center;
    -webkit-justify-content: center;
        -ms-flex-pack: center;
            justify-content: center;
    padding-left: 10px;
    padding-right: 10px;
    text-align: center;
    white-space: nowrap;
  }

  .button:hover {
    border-color: #aeb1b5;
  }

</style>
