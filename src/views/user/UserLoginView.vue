<template>
  <div id="userLoginView">
    <div class="warp">
      <a-layout-header class="header">
        <a-space>
          <img src="../../../public/fly.png" class="logo" />
          <div>AeroCode</div>
        </a-space>
      </a-layout-header>
      <h2 style="margin-bottom: 16px">用户登录</h2>
      <a-form
        style="max-width: 480px; margin: 0 auto"
        label-align="left"
        auto-label-width
        :model="form"
        @submit="handleSubmit"
      >
        <a-form-item field="userAccount" label="账号">
          <a-input v-model="form.userAccount" placeholder="请输入账号" />
        </a-form-item>
        <a-form-item
          field="userPassword"
          tooltip="密码不少于 8 位"
          label="密码"
        >
          <a-input-password
            v-model="form.userPassword"
            placeholder="请输入密码"
          />
        </a-form-item>
        <a-form-item>
          <a-space>
            <a-button type="primary" html-type="submit" style="width: 120px">
              登录
            </a-button>
            <router-link to="/user/register">
              <a-button type="text">注册</a-button>
            </router-link>
          </a-space>
        </a-form-item>
      </a-form>
    </div>
  </div>
</template>

<script setup lang="ts">
import { reactive } from "vue";
import { UserControllerService, UserLoginRequest } from "../../../generated";
import message from "@arco-design/web-vue/es/message";
import { useRouter } from "vue-router";
import { useStore } from "vuex";

/**
 * 表单信息
 */
const form = reactive({
  userAccount: "",
  userPassword: "",
} as UserLoginRequest);

const router = useRouter();
const store = useStore();

/**
 * 提交表单
 * @param data
 */
const handleSubmit = async () => {
  const res = await UserControllerService.userLoginUsingPost(form);
  // 登录成功，跳转到主页
  if (res.code === 0) {
    await store.dispatch("user/getLoginUser");
    router.push({
      path: "/",
      replace: true,
    });
  } else {
    message.error("登陆失败，" + res.message);
  }
};
</script>

<style scoped lang="scss">
.warp {
  width: 700px;
  height: 380px;
  background-color: rgba(255, 255, 255, 0.8);
  margin: 0 auto;
}
.logo {
  width: 45px;
  height: 45px;
}
.header {
  padding-top: 50px;
  margin-top: 5rem;
}
</style>
