<template>
  <el-header class="header">
    <div style="display: flex;justify-content: space-between;margin-top: 6px; ">
      <div style="display: flex;align-items: center;gap: 10px;">
        <img src="@/assets/xiaozhi-logo.png" alt="" style="width: 42px;height: 42px;" />
        <img src="@/assets/xiaozhi-ai.png" alt="" style="width: 58px;height: 12px;" />
        <div class="equipment-management" @click="goHome">
          <img src="@/assets/home/equipment.png" alt="" style="width: 12px;height: 10px;" />
          智能体管理
        </div>
        <div class="console">
          <i class="el-icon-s-grid" style="font-size: 10px;color: #979db1;" />
          控制台
        </div>
        <div class="equipment-management2">
          设备管理
          <img src="@/assets/home/close.png" alt="" style="width: 6px;height: 6px;" />
        </div>
      </div>
      <div style="display: flex;align-items: center;gap: 7px; margin-top: 2px;">
        <div class="serach-box">
          <el-input placeholder="输入名称搜索.." v-model="serach" style="border: none; background: transparent;" @keyup.enter.native="handleSearch" />
          <img src="@/assets/home/search.png" alt=""
               style="width: 14px;height: 14px;margin-right: 11px;cursor: pointer;" @click="handleSearch" />
        </div>
        <img src="@/assets/home/avatar.png" alt="" style="width: 21px;height: 21px;" />
        <div class="user-info">
           {{ userInfo.mobile || '加载中...' }}
        </div>
      </div>
    </div>
  </el-header>
</template>

<script>
import userApi from '@/apis/module/user'


export default {
  name: 'HeaderBar',
  props: ['devices'],  // 接收父组件设备列表
  data() {
    return {
      serach: '',
      userInfo: {
        mobile: ''
      }
    }
  },
  mounted() {
    this.fetchUserInfo()
  },
  methods: {
    goHome() {
      // 跳转到首页
      this.$router.push('/')
    },
    // 获取用户信息
    fetchUserInfo() {
      userApi.getUserInfo(({data}) => {
        this.userInfo = data.data
      })
    },

    // 处理搜索
    handleSearch() {
      const searchValue = this.serach.trim();
      let filteredDevices;

      if (!searchValue) {
        // 当搜索内容为空时，显示原始完整列表
        filteredDevices = this.$parent.originalDevices;
      } else {
         // 过滤逻辑
        filteredDevices = this.devices.filter(device => {
          return device.agentName.includes(searchValue) ||
              device.ttsModelName.includes(searchValue) ||
              device.ttsVoiceName.includes(searchValue);
        });
      }

      this.$emit('search-result', filteredDevices);
    }

  }

}
</script>

<style scoped>
.equipment-management,
.equipment-management2 {
  cursor: pointer;
}

.equipment-management {
  width: 82px;
  height: 24px;
  border-radius: 12px;
  background: #5778ff;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 7px;
  font-weight: 500;
  color: #fff;
  font-size: 10px;
}

.equipment-management2 {
  width: 87px;
  height: 22px;
  border-radius: 11px;
  background: #fff;
  display: flex;
  justify-content: center;
  font-size: 9px;
  font-weight: 400;
  gap: 7px;
  color: #3d4566;
  margin-left: 2px;
  align-items: center;
}

.header {
  background: #f6fcfe66;
  border: 1px solid #fff;
  height: 53px !important;
}

.serach-box {
  display: flex;
  width: 220px;
  height: 30px;
  border-radius: 15px;
  background-color: #f6fcfe66;
  border: 1px solid #e4e6ef;
  align-items: center;
  padding: 0 7px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  margin-right: 15px;
}

.serach-box /deep/ .el-input__inner {
  border-radius: 15px;
  height: 100%;
  width: 100%;
  border: 0;
  background: transparent;
  padding-left: 12px;
}


.user-info {
  font-weight: 600;
  font-size: 12px;
  letter-spacing: -0.02px;
  text-align: left;
  color: #3d4566;
}
.console {
  width: 90px;
  height: 22px;
  border-radius: 11px;
  background: radial-gradient(50% 50% at 50% 50%, #fff 0%, #e8f0ff 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 9px;
  color: #979db1;
  font-weight: 400;
  gap: 7px;
  margin-left: 15px;
}
</style>