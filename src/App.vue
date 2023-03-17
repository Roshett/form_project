<script>
import { defineComponent } from "vue";
import {
  NButton,
  NLayout,
  NLayoutHeader,
  NLayoutSider,
  NInput,
  NForm,
  NFormItem,
} from "naive-ui";

// import html2pdf from 'html2pdf.js';
import html2pdf from "html2pdf.js/dist/html2pdf.bundle.js";

export default defineComponent({
  components: {
    NButton,
    NLayout,
    NLayoutHeader,
    NLayoutSider,
    NInput,
    NForm,
    NFormItem,
  },
  data() {
    return {
      user: {
        number: "",
        firstName: "",
        lastName: "",
      },
    };
  },
  methods: {
    isFormEmpty() {
      return (
        this.user.number === "" &&
        this.user.firstName === "" &&
        this.user.lastName === ""
      );
    },
    submit() {
      if (this.isFormEmpty()) {
        return;
      }

      var element = document.getElementById("element-to-print");
      var opt = {
        margin: 1,
        filename: "form.pdf",
        image: { type: "png", quality: 0.98 },
        html2canvas: { scale: 2 },
        jsPDF: { unit: "in", format: "letter", orientation: "portrait" },
      };

      // New Promise-based usage:
      html2pdf().from(element).set(opt).save();

      // Old monolithic-style usage:
      html2pdf(element, opt);
    },
    cancel() {
      // reset form
      this.user = {
        number: "",
        firstName: "",
        lastName: "",
      };
    },
  },
});
</script>
<template>
  <div style="height: 360px; position: relative">
    <n-layout position="absolute">
      <n-layout-header style="height: 64px; padding: 24px" bordered>
        <h3>Form</h3>
      </n-layout-header>
      <n-layout has-sider position="absolute" style="top: 64px; bottom: 64px">
        <n-layout-sider bordered content-style="padding: 24px;">
          Member ship renewal form<br />
          2023-2024
        </n-layout-sider>
        <n-layout content-style="padding: 24px;">
          <n-form
            inline
            :label-width="80"
            :model="user"
            :rules="rules"
            :size="size"
          >
            <n-form-item label="Member number" path="user.name">
              <n-input v-model:value="user.number" placeholder="4254234" />
            </n-form-item>
            <n-form-item label="First Name" path="user.name">
              <n-input v-model:value="user.firstName" placeholder="Anthony" />
            </n-form-item>
            <n-form-item label="Last Name" path="user.name">
              <n-input v-model:value="user.lastName" placeholder="Kerzen" />
            </n-form-item>
            <!-- create button for submit -->
            <n-form-item>
              <n-button @click="cancel">Cancel</n-button>
              <n-button @click="submit" type="primary" style="margin-left: 10px"
                >Print</n-button
              >
            </n-form-item>
          </n-form>
        </n-layout>
      </n-layout>
      <n-layout-footer
        bordered
        position="absolute"
        style="height: 64px; padding: 24px"
      >
        <n-button>Cancel</n-button>
        <n-button type="primary">Submit</n-button>
      </n-layout-footer>
    </n-layout>
  </div>
  <form id="element-to-print">
    <h3>Form Preview</h3>
    <!-- <div style="background-image: url('https://cdn.pixabay.com/photo/2017/03/16/21/18/logo-2150297__340.png'); width: 100px; height: 100px;"></div> -->
    <p>
      Member ship renewal form<br />
      2023-2024
    </p>
    <p>Member number: {{ user.number }}</p>
    <p>First Name: {{ user.firstName }}</p>
    <p>Last Name: {{ user.lastName }}</p>
  </form>
</template>
