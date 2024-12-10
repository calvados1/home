<template>
  <!-- 功能区域 -->
  <div :class="store.mobileFuncState ? 'function mobile' : 'function'">
    <el-row :gutter="20">
      <el-col :span="12">
        <div class="left">
          <Hitokoto />
          <Music v-if="playerHasId" />
        </div>
      </el-col>
      <el-col :span="12">
        <div class="right cards">
          <div class="time">
            <div class="date">
              <span>{{ currentTime.year }}&nbsp;年&nbsp;</span>
              <span>{{ currentTime.month }}&nbsp;月&nbsp;</span>
              <span>{{ currentTime.day }}&nbsp;日&nbsp;</span>
              <span class="sm-hidden">{{ currentTime.weekday }}</span>
            </div>
            <div class="text time-wrapper">
              <span>{{ currentTime.hour }}</span>
              <span class="colon">:</span>
              <span>{{ currentTime.minute }}</span>
              <span class="colon">:</span>
              <span>{{ currentTime.second }}</span>
            </div>
          </div>
          <Weather />
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script setup>
import { getCurrentTime } from "@/utils/getTime";
import { mainStore } from "@/store";
import Music from "@/components/Music.vue";
import Hitokoto from "@/components/Hitokoto.vue";
import Weather from "@/components/Weather.vue";

const store = mainStore();

// 当前时间
const currentTime = ref({});
const timeInterval = ref(null);

// 播放器 id
const playerHasId = import.meta.env.VITE_SONG_ID;

// 更新时间
const updateTimeData = () => {
  currentTime.value = getCurrentTime();
};

onMounted(() => {
  updateTimeData();
  timeInterval.value = setInterval(updateTimeData, 1000);
});

onBeforeUnmount(() => {
  clearInterval(timeInterval.value);
});
</script>

<style lang="scss" scoped>
.function {
  height: 165px;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  &.mobile {
    .el-row {
      .el-col {
        &:nth-of-type(1) {
          display: contents;
        }
        &:nth-of-type(2) {
          display: none;
        }
      }
    }
  }
  .el-row {
    height: 100%;
    width: 100%;
    margin: 0 !important;
    .el-col {
      &:nth-of-type(1) {
        padding-left: 0 !important;
      }
      &:nth-of-type(2) {
        padding-right: 0 !important;
      }
      @media (max-width: 910px) {
        &:nth-of-type(1) {
          display: none;
        }
        &:nth-of-type(2) {
          padding: 0 !important;
          flex: none;
          max-width: none;
          width: 100%;
        }
      }
    }
    .left,
    .right {
      width: 100%;
      height: 100%;
    }
    .right {
      padding: 20px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: space-between;
      animation: fade 0.5s;
      .time {
        .date {
          text-overflow: ellipsis;
          overflow-x: hidden;
          white-space: nowrap;
          color: #e2a5bc;
          font-size: 1.1rem;
          text-shadow: 0 0 10px rgba(226, 165, 188, 0.5);
          letter-spacing: 1px;
          font-weight: 500;
          text-align: center;
        }

        .time-wrapper {
          margin-top: 10px;
          font-size: 3.5rem;
          font-weight: bold;
          letter-spacing: 2px;
          display: flex;
          justify-content: center;
          align-items: center;
          gap: 0.2rem;
          font-family: 'Josefin Sans', sans-serif;

          span {
            background: linear-gradient(45deg, #ff6eb4 10%, #a05aff 90%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-shadow: 3px 3px 15px rgba(255, 110, 180, 0.3);
            position: relative;
            animation: float 3s ease-in-out infinite;
          }

          .colon {
            font-size: 3rem;
            margin: 0 0.1rem;
            color: #ff6eb4;
            -webkit-text-fill-color: #ff6eb4;
            animation: blink 1.5s ease-in-out infinite;
            opacity: 0.8;
          }

          &:hover span {
            animation-play-state: paused;
            transform: scale(1.1);
            transition: transform 0.3s ease;
          }
        }

        @media (min-width: 1201px) and (max-width: 1280px) {
          .time-wrapper {
            font-size: 3rem;
          }
        }

        @media (min-width: 911px) and (max-width: 992px) {
          .time-wrapper {
            font-size: 2.75rem;
          }
        }
      }
      .weather {
        text-align: center;
        width: 100%;
        text-overflow: ellipsis;
        overflow-x: hidden;
        white-space: nowrap;
      }
    }
  }
}

@keyframes float {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-5px);
  }
}

@keyframes blink {
  0%, 100% {
    opacity: 1;
  }
  50% {
    opacity: 0.3;
  }
}

@keyframes fade {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>