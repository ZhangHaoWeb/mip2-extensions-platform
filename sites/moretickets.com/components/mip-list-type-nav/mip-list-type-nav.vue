<template>
  <mip-fixed
    type="top"
    class="mip-fix-top">
    <div class="list-type-nav-container">
      <div
        v-if="showNav"
        class="list-type-nav">
        <div class="type-nav-container">
          <div
            v-for="(showType,index) in showTypeData"
            :key="index"
            :class="{'active': showType.type === activeType,'opacity': isOpacity}"
            class="type-nav"
            @click="switchType(showType.type)">
            <div class="type-name">{{ showType.title }}</div>
          </div>
        </div>
      </div>
      <div
        v-if="!showNav"
        class="list-type-nav">
        <div class="type-nav-container">
          <div class="type-nav">
            <div class="type-name">“<span style="color:#ff1d41;">{{ keyword }}</span>”相关的演出</div>
          </div>
          <div
            class="type-nav"
            @click="resetKeyword">
            <div
              class="type-name"
              style="color:rgb(168, 166, 167);">清空搜索</div>
          </div>
        </div>
      </div>
    </div>
  </mip-fixed>
</template>

<style scoped lang="less">
*{
  -webkit-tap-highlight-color: transparent;
  outline: none;
}
.mip-fix-top {
  top: 94px !important;
}
@main-color: #ff1d41;
@boder-color: #eaeaeb;
.list-type-nav-container {
  border-bottom: 1px solid @boder-color;
  background: #fff;
}
.list-type-nav {
  overflow-x: auto;
  overflow-y: hidden;
  line-height: 57px;
  .type-nav-container {
    min-width: 100%;
    display: -webkit-box;
    white-space: nowrap;
    display: flex;
    height: 60px;
    -webkit-overflow-scrolling: touch;
    .type-nav {
      padding: 0 1.3rem;
      height: 100%;
      text-align: center;
      font-size: 1.4rem;
      white-space: nowrap;
      display: inline-block;
      flex-shrink: 0;
      opacity: 1;
      &.opacity{
        opacity: 0.99;
      }
      &.active {
        .type-name {
          border-bottom: 3px solid @main-color;
        }
        color: @main-color;
      }
    }
  }
}
</style>

<script>
export default {
  props: {
    prefixUrl: {
      type: String,
      default: ''
    },
    activeType: {
      type: String,
      default: '1'
    },
    keyword: {
      type: String,
      default: ''
    }
  },
  data () {
    return {
      showNav: true,
      isOpacity: false,
      showTypeData: [
        {
          title: '演唱会',
          type: '1'
        },
        {
          title: '话剧歌剧',
          type: '3'
        },
        {
          title: '音乐会',
          type: '2'
        },
        {
          title: '儿童亲子',
          type: '9'
        },
        {
          title: '舞蹈芭蕾',
          type: '5'
        },
        {
          title: '展览休闲',
          type: '7'
        },
        {
          title: '曲艺杂谈',
          type: '4'
        },
        {
          title: '体育赛事',
          type: '6'
        }
      ],
      interval: null,
      times: 0
    }
  },
  mounted () {
    let me = this
    MIP.watch('keyword', newValue => {
      me.showNav = newValue === ''
      me.keyword = newValue
    })
    this.interval = setInterval(function () {
      if (me.times >= 5) {
        clearInterval(me.interval)
        me.isOpacity = false
      }
      me.times += 1
      me.isOpacity = true
    }, 500)
  },
  methods: {
    resetKeyword () {
      MIP.setData({
        keyword: ''
      })
    },
    switchType (type) {
      MIP.setData({
        activeType: type,
        loading: true
      })
      setTimeout(function () {
        if (MIP.getData('loading')) {
          MIP.setData({
            loading: false
          })
        }
      }, 2000)
    }
  }
}
</script>
