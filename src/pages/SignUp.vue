<template>
  <section class="py-24 bg-white overflow-hidden" style="min-height: 60vh">
    <div class="container px-4 mx-auto">
      <div class="flex justify-center flex-wrap -mx-4">
        <div class="w-full md:w-1/2 lg:w-1/3 px-4">
          <article
            class="animate-in flex-1 flex flex-col w-full justify-center gap-2 text-foreground"
          >
            <label class="text-left text-md"> Email </label>
            <input
              v-model="email"
              class="rounded-md px-4 py-2 bg-inherit border mb-6"
              name="email"
              placeholder="you@example.com"
              required
            />
            <label class="text-left text-md"> Password </label>
            <input
              v-model="password"
              class="rounded-md px-4 py-2 bg-inherit border mb-6"
              type="password"
              name="password"
              placeholder="••••••"
              required
            />
            <label class="text-left text-md"> Confirm Password </label>
            <input
              v-model="confirmPassword"
              class="rounded-md px-4 py-2 bg-inherit border mb-6"
              type="password"
              name="password"
              placeholder="••••••"
              required
            />
            <button
              @click="createAccount"
              class="bg-violet-500 text-violet-50 rounded-md px-4 py-2 text-foreground mb-2"
            >
              Create Account
            </button>
            <p
              class="text-center text-sm font-light text-gray-500 dark:text-gray-400"
            >
              Already have an account?
              <router-link
                to="/login"
                class="font-medium text-violet-400 hover:underline dark:text-primary-500"
              >
                Login here
              </router-link>
            </p>
          </article>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref } from "vue";
import { supabase } from "../../util/supabase/supabase";
import { useRouter } from "vue-router";

const UITableName = "userui"; // ui관련 저장DB 테이블명

const email = ref("");
const password = ref("");
const confirmPassword = ref("");
const name = ref("");

const createAccount = async () => {
  console.log("createAccount");

  // 비번과 비번확인 다를때 경고
  if (password.value !== confirmPassword.value) {
    alert("Passwords do not match. Please check your password.");
  } else {
    const { data, error } = await supabase.auth.signUp({
      email: email.value,
      password: password.value,
      // user.user_metadata 객체 아래로 자유롭게 데이터추가 가능
      options: {
        data: {
          first_name: name.value,
        },
      },
    });
    if (error) {
      console.log(error.message);
      alert(error.message);
    } else {
      // 보낸이 : noreply@mail.app.supabase.io
      alert("회원가입 성공! 이메일을 확인하세요.");
      console.log(data);
    }
  }
};
</script>

<style scoped lang="scss"></style>
