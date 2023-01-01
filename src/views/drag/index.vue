<template>
  <div>
    <div
        v-for="(item, index) in list"
        :key="index"
        class="drag-container no-touch"
        :style="{
          left: `${item.initLeft}px`,
          top: `${item.initTop}px`,
          width: `${item.width}`,
          height: `${item.height}`,
          zIndex: `${item.zIndex}`
        }"
        @mousedown="putDown($event,item)"
    >
      <span>{{ item.name }}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'drag',
  data() {
    return {
      list: [
        {
          id: 1001,
          name: '方块1',
          initLeft: 0,
          initTop: 0,
          width: 100,
          height: 100,
          zIndex: 1,
        },
        {
          id: 1002,
          name: '方块2',
          initLeft: 110,
          initTop: 0,
          width: 100,
          height: 100,
          zIndex: 2,
        },
        {
          id: 1003,
          name: '方块3',
          initLeft: 220,
          initTop: 0,
          width: 100,
          height: 100,
          zIndex: 3,
        }
      ]
    }
  },
  methods: {
    putDown(event, item) {
      let initXAxis = item.initLeft
      let initYAxis = item.initTop
      let innerX = event.clientX - initXAxis
      let innerY = event.clientY - initYAxis
      let listObj = this.list.find(listObjItem => listObjItem.id === item.id)
      console.log('event', event)

      // this.changeAllElementZIndex(item.id)
      document.onmousemove = function (event) {
        listObj.initLeft = event.clientX - innerX
        listObj.initTop = event.clientY - innerY
        // 边界判断
        if (parseInt(listObj.initLeft) <= 0) {
          listObj.initLeft = 0
        }
        if (parseInt(listObj.initTop) <= 0) {
          listObj.initTop = 0
        }
        if (
            parseInt(listObj.initLeft) >=
            window.innerWidth - parseInt(listObj.width)
        ) {
          listObj.initLeft =
              window.innerWidth - parseInt(listObj.width)
        }
        if (
            parseInt(item.initTop) >=
                window.innerHeight - parseInt(item.height)
            ) {
          item.initTop =
              window.innerHeight - parseInt(item.height)
        }
      }
      document.onmouseup = function () {
        document.onmousemove = null
        document.onmouseup = null
      }
    },
    changeAllElementZIndex(id) {
      this.list = [...this.list].map(item => {
        if (item.id === id) {
          return {
            ...item,
            zIndex: this.list.length
          }
        } else {
          return {
            ...item,
            zIndex: item.zIndex < 1 ? parseInt(item.zIndex) - 1 : 1
          }
        }
      })
    }
  }
}
</script>

<style scoped>
.drag-container {
  background-color: skyblue;
  position: absolute;
  line-height: 100px;
  text-align: center;
  width: 100px;
  height: 100px;
}

.no-touch {
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}
</style>