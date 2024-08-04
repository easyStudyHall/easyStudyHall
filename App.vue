<template>
  <div class="root">
    <el-container>
      <el-header
        ><img src="./assets/logo.png" style="height: 100%; float: left" />
        <h1>易自习作业管理系统 当前时间：{{ nowDateTime }}</h1>
      </el-header>
      <el-main>
        <el-collapse>
          <el-collapse-item
            title="单击此处编辑作业，每编辑完一项点击“插入”按钮，全部编辑完成之后点击“保存”按钮，再次单击可收起编辑面板"
            name="1"
          >
            <div class="form">
              <el-form>
                <el-form-item label="科目">
                  <el-radio-group v-model="subject">
                    <el-radio label="语文">语文</el-radio>
                    <el-radio label="数学">数学</el-radio>
                    <el-radio label="英语">英语</el-radio>
                    <el-radio label="物理">物理</el-radio>
                    <el-radio label="化学">化学</el-radio>
                    <el-radio label="生物">生物</el-radio>
                    <el-radio label="政治">政治</el-radio>
                    <el-radio label="历史">历史</el-radio>
                    <el-radio label="地理">地理</el-radio>
                    <el-radio label="通知">通知</el-radio>
                  </el-radio-group>
                </el-form-item>
                <el-form-item label="作业类目">
                  <el-radio-group v-model="category">
                    <el-radio label="大本">大本</el-radio>
                    <el-radio label="小本">小本</el-radio>
                    <el-radio label="必刷题">必刷题</el-radio>
                    <el-radio label="卷">卷</el-radio>
                    <el-radio label="报纸">报纸</el-radio>
                    <el-radio label="作文">作文</el-radio>
                    <el-radio label="思考题">思考题</el-radio>
                    <el-radio label="通知">通知</el-radio>
                  </el-radio-group>
                </el-form-item>
                <el-form-item label="作业详情">
                  <el-input
                    v-model="detail"
                    type="textarea"
                    :rows="4"
                  ></el-input>
                </el-form-item>
                <el-form-item>
                  <el-button type="primary" @click="insert">插入</el-button>
                  <el-button type="success" @click="saveOffline"
                    >保存</el-button
                  >
                  <el-button type="warning" @click="reloadPage"
                    >刷新页面</el-button
                  >
                  <el-button type="danger" @click="resetAll"
                    >重置当日作业（此操作不可逆）</el-button
                  >
                  <el-button type="info" @click="settings = true"
                    >设置</el-button
                  >
                </el-form-item>
              </el-form>
            </div>
          </el-collapse-item>
        </el-collapse>
        <el-drawer
          v-model="settings"
          title="系统设置"
          :direction="direction"
          :before-close="handleClose"
        >
          <h4>科目设置：显示的科目</h4>
          <el-form :inline="false">
            <el-form-item>
              <el-checkbox v-model="showChinese">语文</el-checkbox>
            </el-form-item>
            <el-form-item>
              <el-checkbox v-model="showMath">数学</el-checkbox>
            </el-form-item>
            <el-form-item>
              <el-checkbox v-model="showEnglish">英语</el-checkbox>
            </el-form-item>
            <el-form-item>
              <el-checkbox v-model="showPhysics">物理</el-checkbox>
            </el-form-item>
            <el-form-item>
              <el-checkbox v-model="showChemistry">化学</el-checkbox>
            </el-form-item>
            <el-form-item>
              <el-checkbox v-model="showBiology">生物</el-checkbox>
            </el-form-item>
            <el-form-item>
              <el-checkbox v-model="showPolitics">政治</el-checkbox>
            </el-form-item>
            <el-form-item>
              <el-checkbox v-model="showHistory">历史</el-checkbox>
            </el-form-item>
            <el-form-item>
              <el-checkbox v-model="showGeography">地理</el-checkbox>
            </el-form-item>
          </el-form>
          <el-button type="success" @click="saveSettings">保存设置</el-button>
          <el-button type="danger" @click="resetSettings"
            >恢复默认选科设置（物化生）</el-button
          >
        </el-drawer>
        <div class="homeworkPanel">
          <!-- 分别创建6个科目的表格组件,并绑定到对应科目的数组,向子组件传值对应科目的数组 -->
          <!-- 使用2列布局排列下面6个表格组件 -->
          <h4>作业：</h4>
          <el-row :gutter="20">
            <el-col :span="12">
              <editPanel
                :subjectHomework="chinese"
                @deleteItem="handleRemoveItem"
                v-show="showChinese"
              ></editPanel>
              <editPanel
                :subjectHomework="math"
                @deleteItem="handleRemoveItem"
                v-show="showMath"
              ></editPanel>
              <editPanel
                :subjectHomework="english"
                @deleteItem="handleRemoveItem"
                v-show="showEnglish"
              ></editPanel>
            </el-col>
            <el-col :span="12">
              <editPanel
                :subjectHomework="physics"
                @deleteItem="handleRemoveItem"
                v-show="showPhysics"
              ></editPanel>
              <editPanel
                :subjectHomework="chemistry"
                @deleteItem="handleRemoveItem"
                v-show="showChemistry"
              ></editPanel>
              <editPanel
                :subjectHomework="biology"
                @deleteItem="handleRemoveItem"
                v-show="showBiology"
              ></editPanel>
              <editPanel
                :subjectHomework="politics"
                @deleteItem="handleRemoveItem"
                v-show="showPolitics"
              ></editPanel>
              <editPanel
                :subjectHomework="history"
                @deleteItem="handleRemoveItem"
                v-show="showHistory"
              ></editPanel>
              <editPanel
                :subjectHomework="geography"
                @deleteItem="handleRemoveItem"
                v-show="showGeography"
              ></editPanel>
            </el-col>
          </el-row>
          <el-row :gutter="20">
            <h4>通知：</h4>
            <el-col :span="24">
              <editPanel
                :subjectHomework="biology"
                @deleteItem="handleRemoveItem"
              ></editPanel>
            </el-col>
          </el-row>
        </div>
      </el-main>
      <el-footer>
        <!-- 版权信息 -->
        <p>易自习作业管理系统V1.3.1 Released under the Apache License 2.0</p>
        <p>made by Zhao</p>
        <p>2024~present</p>
      </el-footer>
    </el-container>
  </div>
</template>

<script>
import EditPanel from "./components/editPanel.vue";

export default {
  name: "App",
  //引入editPanel组件
  components: {
    EditPanel,
  },
  data() {
    return {
      subject: "",
      category: "",
      detail: "",
      homework: [],
      chinese: [],
      math: [],
      english: [],
      physics: [],
      chemistry: [],
      biology: [],
      politics: [],
      history: [],
      geography: [],
      notice: [],
      settings: false,
      showChinese: true,
      showMath: true,
      showEnglish: true,
      showPhysics: true,
      showChemistry: true,
      showBiology: true,
      showPolitics: false,
      showHistory: false,
      showGeography: false,
      nowDateTime: "",
    };
  },
  methods: {
    /**
     * 插入数据
     *
     * @description 处理插入逻辑，例如发送请求到服务器等。
     *              可以使用 this.subject, this.category, this.detail 来获取表单的值。
     *              插入成功后，将清空表单。
     */
    insert() {
      // 在这里处理插入逻辑，例如发送请求到服务器等。
      // 可以使用 this.subject, this.category, this.detail 来获取表单的值。
      //获取当前时间并格式化为年月日
      let date = new Date();
      let year = date.getFullYear();
      let month = date.getMonth() + 1;
      let day = date.getDate();
      // 拼接时间
      let dateStr = year + "-" + month + "-" + day;
      this.homework.push({
        subject: this.subject,
        category: this.category,
        detail: this.detail,
        created_at: dateStr,
      }) || [];
      // 重新初始化表单
      this.subject = "";
      this.category = "";
      this.detail = "";
    },
    /**
     * 保存到本地
     */
    saveOffline() {
      localStorage.setItem("homework", JSON.stringify(this.homework));
      this.handleInsertItem();
      this.storeSubject();
    },
    /**
     * 处理插入作业项
     * 遍历homework数组，根据科目将作业项插入到对应科目的数组中
     */
    handleInsertItem() {
      this.homework.forEach((item) => {
        switch (item.subject) {
          case "语文":
            this.chinese.push(item);
            break;
          case "数学":
            this.math.push(item);
            break;
          case "英语":
            this.english.push(item);
            break;
          case "物理":
            this.physics.push(item);
            break;
          case "化学":
            this.chemistry.push(item);
            break;
          case "生物":
            this.biology.push(item);
            break;
          case "通知":
            this.notice.push(item);
            break;
          //新增政治历史地理
          case "政治":
            this.politics.push(item);
            break;
          case "历史":
            this.history.push(item);
            break;
          case "地理":
            this.geography.push(item);
            break;
          default:
            break;
        }
      });
    },

    /**
     * 将分科目的数据储存到本地
     */
    storeSubject() {
      localStorage.setItem("chinese", JSON.stringify(this.chinese));
      localStorage.setItem("math", JSON.stringify(this.math));
      localStorage.setItem("english", JSON.stringify(this.english));
      localStorage.setItem("physics", JSON.stringify(this.physics));
      localStorage.setItem("chemistry", JSON.stringify(this.chemistry));
      localStorage.setItem("biology", JSON.stringify(this.biology));
      localStorage.setItem("notice", JSON.stringify(this.notice));
      //新增政治历史地理
      localStorage.setItem("history", JSON.stringify(this.history));
      localStorage.setItem("geography", JSON.stringify(this.geography));
      localStorage.setItem("politics", JSON.stringify(this.politics));
    },
    /**
     * 处理删除作业项
     *
     * @param row 待删除的作业项
     */
    handleRemoveItem(row) {
      console.log(row);
      //根据row查找homework数组中对应的数据，并删除
      this.homework = this.homework.filter((item) => item !== row);
      console.log(this.homework);
      //初始化7科目数组
      this.initSubject;
      // 调用saveOffline方法，将更新后的作业数据保存到本地存储
      this.saveOffline();
      // 重新加载页面，以反映删除操作后的最新数据状态
      location.reload();
    },

    /**
     * 初始化科目
     *
     * @returns 无返回值
     */
    initSubject() {
      this.chinese = [];
      this.math = [];
      this.english = [];
      this.physics = [];
      this.chemistry = [];
      this.biology = [];
      this.notice = [];
      //新增政治历史地理
      this.history = [];
      this.geography = [];
      this.politics = [];
    },

    /**
     * 重置所有作业数据
     *
     * @returns 无返回值
     */
    resetAll() {
      this.homework = [];
      this.initSubject();
      // 调用saveOffline方法，将更新后的作业数据保存到本地存储
      this.saveOffline();
      // 重新加载页面，以反映删除操作后的最新数据状态
      location.reload();
    },

    /**
     * 重新加载页面
     *
     * @returns 无返回值
     */
    reloadPage() {
      location.reload();
    },

    /**
     * 保存设置
     *
     * 将当前组件的多个属性保存到localStorage中，以便下次打开页面时恢复用户设置
     */
    saveSettings() {
      localStorage.setItem("showChinese", this.showChinese);
      localStorage.setItem("showMath", this.showMath);
      localStorage.setItem("showEnglish", this.showEnglish);
      localStorage.setItem("showPhysics", this.showPhysics);
      localStorage.setItem("showChemistry", this.showChemistry);
      localStorage.setItem("showBiology", this.showBiology);
      localStorage.setItem("showHistory", this.showHistory);
      localStorage.setItem("showGeography", this.showGeography);
      localStorage.setItem("showPolitics", this.showPolitics);
    },

    /**
     * 重置设置到物化生
     *
     * @returns 无返回值
     */
    resetSettings() {
      this.showChinese = true;
      this.showMath = true;
      this.showEnglish = true;
      this.showPhysics = true;
      this.showChemistry = true;
      this.showBiology = true;
      this.showPolitics = false;
      this.showHistory = false;
      this.showGeography = false;
      //保存设置并重载页面
      this.saveSettings();
      this.reloadPage();
    },
    getTime() {
      let yy = new Date().getFullYear();
      let mm = new Date().getMonth() + 1;
      let dd = new Date().getDate();
      let hh = new Date().getHours();
      let mf =
        new Date().getMinutes() < 10
          ? "0" + new Date().getMinutes()
          : new Date().getMinutes();
      let ss =
        new Date().getSeconds() < 10
          ? "0" + new Date().getSeconds()
          : new Date().getSeconds();
      this.nowDateTime =
        yy + "年 " + mm + "月" + dd + "日 " + hh + ":" + mf + ":" + ss;
    },
    currentTime() {
      setInterval(this.getTime, 500);
    },
  },
  /**
   * 组件挂载后执行的函数
   */
  mounted() {
    // 组件挂载完成后执行的代码
    console.log("App is mounted.");

    // 从本地存储中获取作业数据，并解析为JSON对象
    this.homework = JSON.parse(localStorage.getItem("homework")) || [];

    // 打印作业数据
    console.log(this.homework);

    // 处理插入作业项的逻辑
    this.handleInsertItem();

    // // 设置定时器，每秒更新时间
    // this.interval = setInterval(() => {
    //   // 更新当前时间
    //   this.currentTime = new Date(); // 更新时间
    // }, 1000);

    //从本地存储中获取用户设置
    this.showChinese = localStorage.getItem("showChinese");
    this.showMath = localStorage.getItem("showMath");
    this.showEnglish = localStorage.getItem("showEnglish");
    this.showPhysics = localStorage.getItem("showPhysics");
    this.showChemistry = localStorage.getItem("showChemistry");
    this.showBiology = localStorage.getItem("showBiology");
    this.showHistory = localStorage.getItem("showHistory");
    this.showGeography = localStorage.getItem("showGeography");
    this.showPolitics = localStorage.getItem("showPolitics");

    //将上述变量转为布尔值
    this.showChinese = this.showChinese === "true";
    this.showMath = this.showMath === "true";
    this.showEnglish = this.showEnglish === "true";
    this.showPhysics = this.showPhysics === "true";
    this.showChemistry = this.showChemistry === "true";
    this.showBiology = this.showBiology === "true";
    this.showHistory = this.showHistory === "true";
    this.showGeography = this.showGeography === "true";
    this.showPolitics = this.showPolitics === "true";
  },
  created() {
    this.currentTime();
  },
  // computed: {
  //   //计算当前时间格式化为年月日时分秒的字符串并自动刷新
  //   /**
  //    * 格式化时间函数
  //    *
  //    * @returns 返回格式化后的时间字符串，格式为"YYYY-MM-DD HH:mm:ss"
  //    */
  //   formattedTime() {
  //     const year = this.currentTime.getFullYear();
  //     const month = String(this.currentTime.getMonth() + 1).padStart(2, "0");
  //     const day = String(this.currentTime.getDate()).padStart(2, "0");
  //     let hours = String(this.currentTime.getHours()).padStart(2, "0");
  //     let minutes = String(this.currentTime.getMinutes()).padStart(2, "0");
  //     let seconds = String(this.currentTime.getSeconds()).padStart(2, "0");

  //     // 拼接成字符串
  //     return `${year}-${month}-${day} ${hours}:${minutes}:${seconds}`;
  //   },
  // },
  /**
   * 在组件销毁前清除定时器
   */
  // beforeUnmount() {
  //   if (this.interval) {
  //     clearInterval(this.interval);
  //   }
  // },
};
</script>

<style>
.el-footer {
  text-align: center;
  color: #8c8c8c;
  font-size: 12px;
}
</style>
