<template>

<!--记住这里的小细节,如果是一个小的组件,想要被传值的话,需要在下面定义一个,interfere  -->

  <van-skeleton title avatar :row="3" :loading="props.loading" v-for="user in props.userList">
    <van-card
        :desc="user.profile"
        :title="`${user.username}（${user.planetCode}）`"
        :thumb="user.avatarUrl"
    >
      <template #tags>
        <van-tag plain type="danger" v-for="tag in user.tags" style="margin-right: 8px; margin-top: 8px">
          {{ tag }}
        </van-tag>
      </template>
      <template #footer>
<!--        <van-button size="mini">联系我</van-button>-->
        <button class="tag-read" data-clipboard-text =user.phone @click="copy(user.phone)">是兄弟就来砍我</button>
      </template>
    </van-card>
  </van-skeleton>
</template>

<script setup lang="ts">
import {UserType} from "../models/user";
import Clipboard from 'clipboard';
import {Toast} from "vant";
import {ref} from "vue";
import myAxios from "../plugins/myAxios";

/*
* 在这里定义组件的属性
* */
interface UserCardListProps {
  loading: boolean;
  userList: UserType[];
}
/*
*
* 在这里为属性设置默认值
* */
const props = withDefaults(defineProps<UserCardListProps>(), {
  loading: true,
  // @ts-ignore
  userList: [] as UserType[],
});

// const copy = (userphone :string) => {
//   let clipboard = new Clipboard('.tag-read')
//   clipboard.on('success', e => {
//
//
//     Toast.success('复制电话号成功');
//     // 释放内存
//     clipboard.destroy()
//   })
//   clipboard.on('error', e => {
//     // 不支持复制
//     console.log('该浏览器不支持自动复制')
//     // 释放内存
//     clipboard.destroy()
//   })
// }
const user = ref({
  phone:""
});
const copy = async (userid:string) => {
  try {
    await navigator.clipboard.writeText(userid);
    // 可以在这里添加复制成功的提示
    Toast.success("快去和他一起攻沙")
  } catch (err) {
    console.error('Failed to copy phone number: ', err);
  }
};

</script>

<style scoped>

</style>
