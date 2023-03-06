<template>
  <div class="common-layout">
    <div class="content"><slot /></div>
    <page-footer />
  </div>
</template>

<script>
import PageFooter from '@/layout/footer/PageFooter'

export default {
  name: 'CommonLayout',
  components: { PageFooter }

}
</script>

<style scoped lang="scss">
.common-layout{
  display: flex;
  flex-direction: column;
  height: 100vh;
  overflow: auto;
  background-color: #f0f2f5;
  background-image: url('https://gw.alipayobjects.com/zos/rmsportal/TVYTbAXWheQpRcWDaDMu.svg');
  background-repeat: no-repeat;
  background-position-x: center;
  background-position-y: 110px;
  background-size: 100%;
  .content{
    padding: 32px 0;
    flex: 1;
    @media (min-width: 768px){

      padding: 112px 0 24px;
    }
  }
}
</style>
