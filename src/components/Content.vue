<template>
  <div>
    <div class="artical">
      <h3>工具介绍：</h3>
      <p>本工具可以将宽高像素相同的逐帧图一键生成为连续帧的雪碧图（如果宽高不相同，将会以第一张图的宽高像素作为基准）</p>
      <h3>使用说明：</h3>
      <p>在输入框中分别输入单张图片的宽高像素，将已经命名规范的逐帧图一次全部上传（再次上传会覆盖之前上传的内容）。点击生成图片，右键保存图片即可！</p>
    </div>
    <div class="canvas-container">
      <canvas id="canvas" class="canvas shadow" :width="pixel_width" :height="pixel_height"></canvas>
      <div>
        <p></p>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'Content',

  data() {
    return {
      lineNum: 10,
    };
  },

  computed: {
    pixel_width() {
      const width = this.files.length > this.lineNum
      ? this.pixel.width * this.lineNum
      : this.pixel.width * this.files.length;

      return width;
    },

    pixel_height() {
      const height = this.pixel.height * Math.ceil(this.files.length / this.lineNum);
      return height;
    },

    single_width() {
      return this.pixel.width;
    },

    single_height() {
      return this.pixel.height;
    },
  },

  props: {
    files: {
      type: Array,
      required: false,
      default() {
        return [];
      },
    },

    pixel: {
      type: Object,
      required: false,
      default() {
        return {};
      },
    },

    line_num: {
      type: Number,
      required: false,
      default: 10,
    },

    single_row: {
      type: Boolean,
      required: false,
      default: false,
    },
  },

  watch: {
    files() {
      this.$nextTick(() => {
        this.draw();
      });
    },

    pixel() {
      this.$nextTick(() => {
        this.draw();
      });
    },

    line_num(val) {
      this.lineNum = val;
      this.$nextTick(() => {
        this.draw();
      });
    },

    single_row(val) {
      if (val) {
        this.lineNum = 9999;
      }
      this.$nextTick(() => {
        this.draw();
      });
    },
  },

  methods: {
    draw() {
      const canvas = document.getElementById('canvas');
      const ctx = canvas.getContext('2d');

      const line = this.files.length < this.lineNum ? this.files.length : this.lineNum;
      const row = Math.ceil(this.files.length / this.lineNum);

      this.files.forEach((val, index) => {
        const w = index % this.lineNum;
        const h = Math.floor(index / this.lineNum);

        ctx.drawImage(val, this.single_width * w, this.single_height * h,
          this.single_width, this.single_height);
      });
    },
  },
};
</script>

<style>
  .artical{

    h3{
      font-size: 20px;
      color: #000;
    }
  }

  .canvas-container{

    .canvas{
      max-width: 100%;
    }
  }
</style>
