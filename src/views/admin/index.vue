<template>
  <div class="page">
    <div class="left" :style="!collapsed?'width:200px':''">
      <div class="ltop">
        <h3 v-show="collapsed" class="logo">图书网</h3>
        <h3 v-show="!collapsed" class="logo">{{ title }}</h3>
      </div>
      <el-menu
        default-active="1"
        :collapse="collapsed"
        :collapse-transition="false"
      >
        <el-menu-item index="1">
          <el-icon><setting /></el-icon>
          <span>图书管理</span>
        </el-menu-item>
        <el-menu-item index="2">
          <el-icon><setting /></el-icon>
          <el-button type="primary" link @click="selectFolder">选择封面</el-button>
        </el-menu-item>
      </el-menu>
    </div>
    <div class="right">
      <div class="r_top">
        <div class="r_t_left">
          <el-button link @click="changeCollapsed">
            <el-icon v-show="collapsed" size="18"><Expand /></el-icon>
            <el-icon v-show="!collapsed" size="18"><Fold /></el-icon>
          </el-button>
          <span class="pageName">{{ pageName }}</span>
        </div>
        <div class="r_t_right">
          <el-dropdown placement="bottom" trigger="click">
            <el-button link>
              <el-icon size="18px"><UserFilled /></el-icon>
              <span>{{ username }}</span>
              <el-icon style="margin-left: 5px"><ArrowDownBold /></el-icon>
            </el-button>
            <template #dropdown>
              <el-dropdown-menu>
                <el-dropdown-item @click="clickHandler"
                  >退出登录</el-dropdown-item
                >
              </el-dropdown-menu>
            </template>
          </el-dropdown>
        </div>
      </div>
      <div class="r_bottom">
        <div class="r_b_box">
          <!-- 移除原选择图片文件夹按钮 -->
          <!-- 修改后的图片显示区域 -->
          <div class="cover-display-scrollable">
            <div class="cover-display-grid">
              <div v-for="(cover, index) in covers" :key="index" class="cover-item">
                <img :src="cover.url" alt="封面">
                <span class="cover-name">{{ cover.name }}</span>
              </div>
            </div>
          </div>
          <RouterView />
        </div>
      </div>
    </div>
  </div>
</template>
<script setup>
import { ref, onMounted, watch } from "vue";
import { RouterView, useRouter } from "vue-router";
const router = useRouter();
let title = ref(import.meta.env.VITE_APP_WEB_NAME + "后台管理系统");
const username = ref("");
const pageName = ref("");
const collapsed = ref(false);
// 存储封面图片的 URL
const covers = ref([]);

watch(
  () => router.currentRoute.value,
  (newValue, oldValue) => {
    pageName.value = newValue.meta.title;
  },
  { immediate: true }
);

const changeCollapsed = () => {
  collapsed.value = !collapsed.value;
};

onMounted(() => {
  username.value = localStorage.getItem("username");
});

const clickHandler = () => {
  localStorage.clear();
  router.push({ path: "/login" });
};

// 选择文件夹的函数
const selectFolder = async () => {
  try {
    // 检查浏览器是否支持 File System Access API
    if ('showDirectoryPicker' in window) {
      const directoryHandle = await window.showDirectoryPicker();
      const files = [];
      for await (const entry of directoryHandle.values()) {
        if (entry.kind === 'file' && /\.(jpg|jpeg|png|gif)$/i.test(entry.name)) {
          const file = await entry.getFile();
          const url = URL.createObjectURL(file);
          files.push({ name: entry.name, url });
        }
      }
      // 对文件列表按文件名中的数字排序
      files.sort((a, b) => {
        const numA = parseInt(a.name.match(/\d+/), 10);
        const numB = parseInt(b.name.match(/\d+/), 10);
        return numA - numB;
      });
      covers.value = files;
    } else {
      alert('您的浏览器不支持 File System Access API，请使用最新版本的 Chrome 或 Edge 浏览器。');
    }
  } catch (error) {
    console.error('选择文件夹时出错:', error);
  }
};
</script>
<style scoped lang="scss">
.page {
  height: 100vh;
  width: 100%;
  display: flex;

  .left {
    .ltop {
      height: 56px;
      line-height: 56px;
      border-bottom: 1px solid #e8e8e8;
    }
    .logo {
      margin-left: 0;
      text-align: center;
      color: #409eff;
      width: 100%;
      font-size: 16px;
      font-weight: bold;
    }
    .el-menu {
      border-right: none;
    }
    .el-menu-item {
      height: 46px;
      line-height: 46px;
    }
    .el-menu-item.is-active {
      background-color: #ecf5ff;
    }
  }

  .right {
    flex: 1;
    background-color: #f6f6f6;

    .r_top {
      height: 56px;
      width: 100%;
      background-color: #fff;
      border-bottom: 1px solid #e8e8e8;
      display: flex;
      align-items: center;
      padding: 0 20px;
      justify-content: space-between;

      .r_t_left {
        display: flex;
        align-items: center;

        .pageName {
          margin-left: 10px;
          font-size: 14px;
        }
      }
    }

    .r_bottom {
      height: calc(100vh - 56px);
      width: 100%;
      box-sizing: border-box;
      overflow: hidden;
      overflow-y: auto;

      .r_b_box {
        width: calc(100% - 20px);
        margin: 10px;
        background-color: #fff;
      }
      // 修改后的图片显示区域样式
      .cover-display-scrollable {
        max-height: 450px; // 限制显示区域高度
        overflow-y: auto; // 允许垂直滚动
        margin-top: 10px;
      }
      .cover-display-grid {
        display: grid;
        grid-template-columns: repeat(5, 1fr); // 每行显示五张图
        gap: 10px;
        padding: 10px;
      }
      .cover-item {
        text-align: center;
      }
      .cover-name {
        display: block;
        margin-top: 5px;
        font-size: 12px;
        color: #666;
      }
      .cover-display-grid img {
        max-width: 100%;
        max-height: 200px;
        object-fit: cover;
      }
    }
  }
}
</style>
