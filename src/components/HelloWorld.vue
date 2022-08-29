<template>
  <v-container class="d-flex justify-center">
    <v-form ref="form" lazy-validation>
      <v-card style="max-width: 1024px">
        <v-card-text>
          <v-row class="text-center">
            <v-col cols="12">
              <v-img
                :src="require('../assets/logo.svg')"
                class="my-3"
                contain
                height="200"
              />
            </v-col>
            <v-col class="mb-4">
              <h1 style="margin-bottom: 10px">choose git type</h1>
              <div style="min-width: 815px">
                <v-select
                  v-model="select"
                  :items="type"
                  label="choose git type"
                  outlined
                  :hint="`${select.value}, ${select.annotation}`"
                  item-text="annotation"
                  item-value="value"
                  return-object
                ></v-select>
              </div>
            </v-col>
          </v-row>
          <v-divider class="mb-6" v-if="select.value != ''"></v-divider>

          <v-row v-if="select.value != ''">
            <v-col cols="1">
              <v-subheader>{{ select.value }}</v-subheader>
            </v-col>
            <v-col cols="11">
              <v-text-field
                outlined
                v-model="title"
                label="type something for title..."
                :rules="[(v) => !!v || '此項目為必填']"
              ></v-text-field>
            </v-col>
          </v-row>
          <v-row v-if="select.value != ''">
            <v-col cols="1">
              <v-subheader>body</v-subheader>
            </v-col>
            <v-col cols="11">
              <v-textarea
                v-model="body"
                outlined
                name="body"
                label="type something about commit message"
                value="The Woodman set to work at once, and so sharp was his axe that the tree was soon chopped nearly through."
                :height="300"
              ></v-textarea>
            </v-col>
          </v-row>
          <v-row v-if="select.value != ''">
            <v-col cols="1">
              <v-subheader>footer</v-subheader>
            </v-col>
            <v-col cols="11">
              <v-text-field
                outlined
                v-model="footer"
                label="type something for issue..."
              ></v-text-field>
            </v-col>
          </v-row>

          <v-divider class="mb-6" v-if="select.value != ''"></v-divider>

          <v-row v-if="select.value != ''">
            <v-col cols="12" class="d-flex justify-center">
              <v-btn type="button" @click="submit">build command</v-btn>
            </v-col>
          </v-row>
          <v-row v-if="select.value != ''">
            <v-col col="12">
              <v-textarea
                :disabled="false"
                v-model="command"
                no-resize
                filled
                readonly
              ></v-textarea>
            </v-col>
          </v-row>
        </v-card-text>
      </v-card>
    </v-form>
  </v-container>
</template>

<script>
export default {
  name: "HelloWorld",

  data: () => ({
    Valid: false,
    select: { value: "", annotation: "" },
    title: "",
    body: "",
    footer: "",
    command: "",
    type: [
      { text: "feat", value: "feat", annotation: "新增/修改功能 (feat)" },
      { text: "fix", value: "fix", annotation: "修補 bug (fix)" },
      { text: "docs", value: "docs", annotation: "文件 (docs)" },
      {
        text: "style",
        value: "style",
        annotation: "格式 (style)",
      },
      {
        text: "refactor",
        value: "refactor",
        annotation: "重構 (refactor)",
      },
      {
        text: "perf",
        value: "perf",
        annotation: "改善效能 (perf)",
      },
      {
        text: "test",
        value: "test",
        annotation: "增加測試 (test)",
      },
      {
        text: "chore",
        value: "chore",
        annotation: "建構程序或輔助工具的變動 (chore)",
      },
      {
        text: "revert",
        value: "revert",
        annotation: "撤銷回覆先前的 commit (revert)",
      },
    ],
  }),
  methods: {
    message() {
      if (this.select.value == "") {
        console.log("pls choose one");
      } else if (this.select.value == "feat") {
        console.log("is feat");
        this.body = "因應新需求做調整：\n\
1.\n\
調整項目：\n\
1.";
      } else if (this.select.value == "fix") {
        console.log("is fix");
        this.body = "問題：\n\
1.  \n\
原因： \n\
1.  \n\
調整項目：\n\
1. \n ";
      } else if (this.select.value == "docs") {
        console.log("is docs");
        this.body = "說明事項：\n";
      } else if (this.select.value == "style") {
        console.log("is style");
        this.body = "說明原因：\n\
1.\n\
調整項目：\n\
1.\n ";
      } else if (this.select.value == "refactor") {
        console.log("is refactor");
        this.body = "說明事項：\n\
        \n\
        調整內容：\n\
        1.\n";
      } else if (this.select.value == "perf") {
        console.log("is perf");
        this.body = "說明：\n\
\n\
調整方式：\n ";
      } else if (this.select.value == "test") {
        console.log("is test");
        this.body = "說明：\n";
      } else if (this.select.value == "chore") {
        console.log("is chore");
        this.body = "調整項目：\n1.\n";
      } else if (this.select.value == "revert") {
        console.log("is revert");
        this.body = "type(scope): subject (回覆版本：xxxx)";
      }
    },
    submit() {
      console.log("submit");
      let checkValid = this.$refs.form.validate();
      if (checkValid) {
        console.log("通過");
        this.Valid = checkValid;
        let string =
          "git commit -m '" +
          this.select.value +
          "：" +
          this.title +
          "\n " +
          this.body +
          "\n " +
          this.footer +
          " '";
        console.log(string);
        this.command = string;
      }
    },
  },
  watch: {
    select: {
      handler: function (newValue, oldValue) {
        if (newValue != oldValue) {
          this.message();
        }
      },
      deep: true,
    },
  },
};
</script>