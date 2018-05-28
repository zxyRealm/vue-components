<template>
    <div
      ref="dragWrap"
      @dragenter.stop.prevent="_handleDragEnter"
      @dragleave.stop.prevent="_handleDragLeave"
      @dragover.stop.prevent="_handleDragOver"
      @drop.stop.prevent="_handleDrop"
      class="free-drag-wrap">
      <canvas
        class="drag-canvas"
        @mouseup.stop.prevent="_handlePointerEnd"
        @mousedown.stop.prevent="_handlePointerDown"
        @mousemove.stop.prevent="_handlePointerMove"
        width="200px"
        height="200px"
        :style="position"
      ></canvas>
    </div>
</template>

<script>
    export default {
      name: "index",
      props:{
          value:{
            type:[Object,Array],
            default:[]
          }
      },
      data(){
          return {
            dragStart:false,
            position:{
              left:'50%',
              top:'20px'
            },
            initPost:{
              disX:'',
              disY:''
            }
          }
      },
      mounted(){
        this.position.left = this.$refs.dragWrap.clientWidth/2
      },
      methods:{
        _handlePointerDown(evt){
          this.dragStart = true;
          console.log(evt.target.offsetLeft,evt.target.offsetTop);
          this.initPost = {
            disX:evt.clientX - evt.target.offsetLeft,
            disY:evt.clientY - evt.target.offsetTop,
            minX:evt.target.offsetWidth/2,
            maxX:this.$refs.dragWrap.clientWidth - evt.target.offsetWidth/2,
            maxY:this.$refs.dragWrap.clientHeight - evt.target.offsetHeight
          };
          // console.log(this.$refs.dragWrap.clientWidth);
          console.log('mouse down',this.initPost.maxX,this.initPost.maxY)
        },
        _handlePointerMove(evt){
          if(this.dragStart){
            let base = this.initPost;
            let shiftLeft = evt.clientX - base.disX;
            let shiftTop = evt.clientY - base.disY;
            shiftLeft = shiftLeft<base.minX?base.minX:(shiftLeft>base.maxX?base.maxX:shiftLeft);
            shiftTop = shiftTop<0?0:(shiftTop>base.maxY?base.maxY:shiftTop);
              this.position = {
                left:shiftLeft + 'px',
                top: shiftTop + 'px'
              }
          }
        },
        _handlePointerEnd(evt){
          this.dragStart = false;
          // 返回移动框是个坐标点，左上角为起点，顺时针方向
          let w = evt.target.offsetWidth;  //移动框宽
          let h = evt.target.offsetHeight; //移动框高
          let l = Number(this.position.left.replace('px',''));  // 移动框左上角x坐标
          let t = Number(this.position.top.replace('px',''));    // 移动框左上角y坐标
          let coordinate = [];
          coordinate[0] = l +','+ t;
          coordinate[1] = l+w +','+ t;
          coordinate[2] = l +','+ (t+h);
          coordinate[3] = l+w +','+ (t+h);
          console.log(coordinate);
          this.$emit('input',coordinate)
        },
        _handleDragEnter (evt) {
          console.log('enter',evt)
          // this.emitNativeEvent(evt)
          // if (this.passive) return
          // if (this.disabled || this.disableDragAndDrop || !u.eventHasFile(evt)) return
          // if (this.hasImage() && !this.replaceDrop) return
          // this.fileDraggedOver = true
        },
        _handleDragLeave (evt) {
          console.log('leave',evt)
          // this.emitNativeEvent(evt)
          // if (this.passive) return
          // if (!this.fileDraggedOver || !u.eventHasFile(evt)) return
          // this.fileDraggedOver = false
        },
        _handleDragOver (evt) {
          console.log('over',evt)
          // this.emitNativeEvent(evt)
        },
        _handleDrop (evt) {
          // this.emitNativeEvent(evt)
          // if (this.passive) return
          // if (!this.fileDraggedOver || !u.eventHasFile(evt)) return
          // if (this.hasImage() && this.replaceDrop) {
          //   this.remove()
          // }
          // this.fileDraggedOver = false
          // let file
          // let dt = evt.dataTransfer
          // if (!dt) return
          // if (dt.items) {
          //   for (var i = 0, len = dt.items.length; i < len; i++) {
          //     let item = dt.items[i]
          //     if (item.kind == 'file') {
          //       file = item.getAsFile()
          //       break
          //     }
          //   }
        //   } else {
        //     file = dt.files[0]
        //   }
        //   if (file) {
        //     this._onNewFileIn(file)
        //   }
        },
      }
    }
</script>

<style scoped>
.free-drag-wrap{
  position: relative;
  width:600px;
  height: 500px;
  border:1px solid #ddd;
  margin: 50px auto;
}
  .drag-canvas{
    position: absolute;
    border:1px solid #ddd;
    background: sandybrown;
    cursor: move;
    transform: translateX(-50%);
  }
</style>
