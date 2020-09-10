<template>
  <div>
    <el-upload
      action="http://mall-file-oss.oss-cn-shenzhen.aliyuncs.com"
      :data="dataObj"
      list-type="picture"
      :multiple="false"
      :before-upload="beforeUpload"
      :on-success="handleUploadSuccess"
    >
      <el-button size="small" type="primary"  @click="clickUpLoad">点击上传</el-button>
      <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过10MB</div>
    </el-upload>
  </div>
</template>

<script>
import { getUUID } from "@/utils";
export default {
  //import 引入的组件需要注入到对象中才能使用
  components: {},
  data() {
    //这里存数据
    return {
      dataObj: {
        policy: "",
        signature: "",
        key: "",
        ossaccessKeyId: "",
        dir: "",
        host: "",
        // callback:'',
      },
    };
  },
  //计算属性
  computed: {},
  //监控data中数据变化
  watch: {},
  //方法
  methods: {
    clickUpLoad() {
      this.$http({
        url: this.$http.adornUrl("/third/oss/policy"),
        method: "get",
        params: this.$http.adornParams({}),
      }).then(({ data }) => {
        this.dataObj = data.data;
        this.dataObj.key = data.data.dir + getUUID() + "_";
        console.log("request",this.dataObj);
      });
    },
    // beforeUpload: async function (file) {
    //   await this.$http({
    //     url: this.$http.adornUrl("/third/oss/policy"),
    //     method: "get",
    //     params: this.$http.adornParams({}),
    //   }).then(({ data }) => {
    //     this.dataObj = data.data;
    //     this.dataObj.key = data.data.dir + getUUID() + "_" + file.name;
    //   });
    //   return true;
    // },
    beforeUpload(file) {
      this.dataObj.key = this.dataObj.key + file.name;
      
      return true;
    },
    handleUploadSuccess(res, file) {
      console.log("上传成功...");
      this.$emit("input", this.dataObj.host+"/"+this.dataObj.key);
      console.log(this.dataObj.host+"/"+this.dataObj.key);
    },
  },
  //声明周期 - 创建完成（可以访问当前this实例）
  created() {},
  //生命周期 - 挂载完成（可以访问DOM元素）
  mounted() {},
  beforeCreate() {}, //生命周期 - 创建之前
  beforeMount() {}, //生命周期 - 挂载之前
  beforeUpdate() {}, //声明周期 - 更新之前
  updated() {}, //生命周期 - 更新之后
  beforeDestroy() {}, //生命周期 - 销毁之前
  destroyed() {}, //生命周期 - 销毁之后
  activated() {}, //缓存keep-alive
};
</script>

<style scoped>
</style>