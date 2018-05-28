<template>
  <div class="ip-input-wrap" :style="{ height:height,lineHeight:height }">
    <template v-for="(item,index) in ipAddress">
      <input
        class="ip-input-item"
        type="text"
        ref="ipInput"
        :style="{height: height,lineHeight:height}"
        @blur="setDefaultValue(item,index,$event)"
        @focus="setDefaultValue(item,index,$event)"
        @keyup="turnIpPost(item,index,$event)"
        @input="checkIpValue(item,index)"
        v-model="item.value">
      <span>·</span>
    </template>
  </div>
</template>

<script>
  export default {
    name: 'ip-index',
    props: {
      value: {
        type: [String],
        default: ''
      },
      height: {
        value: {
          type: [String],
          default: '40px'
        }
      },
      autoFill: {
        type: [Boolean],
        default: true
      }
    },
    data() {
      return {
        ipAddress: [
          {value: ''},
          {value: ''},
          {value: ''},
          {value: ''}
        ],
        styleCss: {
          height: this.height,
          lineHeight: this.height
        }
      }
    },
    mounted() {
      const ipArr = this.value.split('.');
      const ipObj = [];
      console.log('初始化', ipArr);
      if (ipArr.length === 4) {
        ipArr.map((item, index) => {
          ipObj[index] = {value: item}
        });
        this.ipAddress = ipObj
      }
    },
    methods: {
      // 校验输入ip格式
      checkIpValue(item) {
        //确保每个值都处于0-255
        let val = item.value;
        //当输入的是空格时，值赋为空
        val = val.trim();
        val = parseInt(val, 10);
        if (isNaN(val)) {
          val = ''
        } else {
          val = val < 0 ? 0 : val;
          val = val > 255 ? 255 : val;
        }
        item.value = val;
      },
      // 根据输入结果切换光标
      turnIpPost(item, index, event) {
        let self = this;
        let e = event || window.event;
        //左箭头向左跳转，删除键把当前数据删除完毕后会跳转到前一个input，左一不做任何措施
        if (e.keyCode === 37 || e.keyCode === 8) {
          if (index !== 0 && (item.value !== '' || this.autoFill)) {
            self.$refs.ipInput[index - 1].focus();
          }
        }

        //右箭头、回车键、空格键、冒号均向右跳转，右一不做任何措施
        if (e.keyCode === 39 || e.keyCode === 13 || e.keyCode === 32 || e.keyCode === 190 || e.keyCode === 110) {
          if (index < 3 && (item.value !== '' || this.autoFill)) {
            self.$refs.ipInput[index + 1].focus();
          }
        }

      },
      setDefaultValue(item, index, event) {
        //当input失去焦点，而ip没有赋值时，会自动赋值为0
        let e = event || window.event;
        let val = item.value.trim();
        if (e.type === 'blur') {
          if (val === '' && this.autoFill) {
            item.value = '0';
          } else if (!this.autoFill && val === '') {
            this.$refs.ipInput[index].focus();
            this.$emit("input", '');
          }
        } else {
          console.log(e.type)
        }
      }
    },
    watch: {
      ipAddress: {
        handler: function (val) {
          let ipStr = '';
          for (let item of val) {
            ipStr += item.value + '.';
            // 当数值超出0-255范围时视为非法ip地址，此时返回空
            if (item.value === '' || Number(item.value)>255 || Number(item.value)<0 || !Number(item.value) ) {
              ipStr = '';
              return
            }
          }
          this.$emit("input", ipStr.slice(0, -1));
        },
        deep: true
      }
    }
  }
</script>

<style scoped>
  .ip-input-wrap {
    width: 100%;
    height: 40px;
    line-height: 40px;
  }

  .ip-input-wrap > .ip-input-item {
    min-width: 40px;
    float: left;
    width: 22%;
    height: 40px;
    line-height: 40px;
    border: 1px solid #dcdfe6;
    text-align: center;
    border-radius: 4px;
    font-size: 14px;
    box-sizing: border-box;
  }

  .ip-input-wrap > .ip-input-item:focus {
    border-color: #409EFF;
    outline: none;
  }

  .ip-input-wrap span {
    float: left;
    width: 4%;
    text-align: center;
  }

  .ip-input-wrap span:last-child {
    display: none;
  }

</style>
