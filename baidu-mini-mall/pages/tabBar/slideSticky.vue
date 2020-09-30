<template>
  <view id="course_container">
    <view class="course-video">我是video的容器</view>
    <view class="course-title">我是课程内容的标题容器</view>
    <view class="course-container">
      <view :class="navBarFixed===true?'isfixed course-nav':'course-nav'" id="mySticky">
        <view
          v-for="(item,index) of tabbarList"
          :key="index"
          :class="activeIndex===index?'course-nav-item active':'course-nav-item'"
          @tap="handleTap(index)"
        >
          {{item}}
          <view class="course-nav-line"></view>
        </view>
      </view>
      <view>
        <course-introduction></course-introduction>
        <course-catalogue></course-catalogue>
      </view>
    </view>
  </view>
</template>

<script>
import CourseIntroduction from './course-introduction'
import CourseCatalogue from './course-catalogue'
export default {
  name: 'Tabbar',
  data () {
    return {
      tabbarList: ['课程介绍', '课表目录'],
      activeIndex: 0,
      navBarFixed: false
    }
  },
  methods: {
    handleTap (index) {
      if (index !== this.activeIndex) {
        this.activeIndex = index
      }
      this.scrollTabbar(index)
    },
    scrollTabbar (index) {
      switch (index) {
        case 0:
          swan.createSelectorQuery().select("#course_container").boundingClientRect(rect => {
            swan.createSelectorQuery().select("#course_introduction").boundingClientRect(res => {
              swan.pageScrollTo({
                duration: 0,
                scrollTop: res.top - rect.top - 50
              })
            }).exec()
          }).exec()
          break
        case 1:
          swan.createSelectorQuery().select("#course_container").boundingClientRect(rect => {
            swan.createSelectorQuery().select("#course_catalogue").boundingClientRect(res => {
              swan.pageScrollTo({
                duration: 0,
                scrollTop: res.top - rect.top - 50
              })
            }).exec()
          }).exec()
          break
        default:
          swan.createSelectorQuery().select("#course_container").boundingClientRect(rect => {
            swan.createSelectorQuery().select("#course_introduction").boundingClientRect(res => {
              swan.pageScrollTo({
                duration: 0,
                scrollTop: res.top - rect.top - 50
              })
            }).exec()
          }).exec()
          break
      }
    }
  },
  components: {
    CourseIntroduction,
    CourseCatalogue
  },
  onPageScroll (object) {
    let scrollTop = object.scrollTop //实时滚动的高度值
    let _this = this
    swan.createSelectorQuery().select("#mySticky").boundingClientRect(rect => {
      let offsetTop = rect.top //距离顶端的位置高度
      if (scrollTop > offsetTop) {
        _this.navBarFixed = true
      } else {
        _this.navBarFixed = false
      }
    }).exec()
    swan.createSelectorQuery().select("#course_catalogue").boundingClientRect(rect => {
      let offsetTop = rect.top //距离顶端的位置高度
      if (offsetTop <= 450) {
        _this.activeIndex = 1
      } else {
        _this.activeIndex = 0
      }
    }).exec()
  }
}
</script>

<style scoped>
#course_container {
  width: 100%;
  height: 500upx;
}

.course-video {
  width: 100%;
  height: 430upx;
  background: #eeeeee;
}

.course-title {
  width: 100%;
  height: 316upx;
  background: #ddd;
}
.course-container {
  width: 100%;
}

.course-nav {
  width: calc(100% - 60upx);
  height: 100upx;
  padding: 0upx 30upx;
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  align-items: center;
}

.course-nav-item {
  display: inline-block;
  margin-right: 60upx;
  position: relative;
  line-height: 100upx;
}
.active {
  color: #00d789;
}

.active .course-nav-line {
  width: 52upx;
  border-bottom: 6upx solid #00d789;
  border-radius: 20upx;
  position: absolute;
  left: 50%;
  margin-left: -26upx;
  bottom: 0upx;
}

.isfixed {
  position: -webkit-sticky;
  position: sticky;
  top: 0upx;
  z-index: 99;
  background-color: #fff;
}

.course-main-container {
  width: 100%;
}
</style>