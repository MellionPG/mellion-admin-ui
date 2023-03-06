<template>
  <div class="footer">
    <div class="links">
      <a v-for="(item, index) in linkList" :key="index" target="_blank" :href="item.link ? item.link : 'javascript: void(0)'">
        {{ item.name }}
      </a>
    </div>
    <div class="copyright">
      Copyright © {{ copyright }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'PageFooter',
  data() {
    return {
      linkList: [
        {
          name: '首页',
          link: 'https://github.com/MellionPG'
        },
        {
          name: 'Github',
          link: 'https://github.com/MellionPG/mellion-admin'
        },
        {
          name: 'MellionPG',
          link: 'https://github.com/MellionPG'
        }
      ],
      copyright: '2023 MellionPG开发'
    }
  }
}
</script>

<style scoped lang="scss">
.footer{
    padding: 48px 16px 24px;
    /*margin: 48px 0 24px;*/
    text-align: center;
    .copyright{
      color: rgba(0, 0, 0, 0.45);
      font-size: 14px;
      i {
          margin: 0 4px;
      }
    }
    .links{
      margin-bottom: 8px;
      a:not(:last-child) {
        margin-right: 40px;
      }
      a{
        color: rgba(0, 0, 0, 0.45);
        -webkit-transition: all .3s;
        transition: all .3s;
      }
    }
  }
</style>
