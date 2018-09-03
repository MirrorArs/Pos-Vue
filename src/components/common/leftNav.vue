<template>
  <div class="left-nav">
    <ul>
      <li>
        <svg class="icon" aria-hidden="true">
          <use xlink:href="#icon-shouyin"></use>
        </svg>
        <div>收银</div>
      </li>
      <li>
        <svg class="icon" aria-hidden="true">
          <use xlink:href="#icon-dianpu"></use>
        </svg>
        <div>店铺</div>
      </li>
      <li>
        <svg class="icon" aria-hidden="true">
          <use xlink:href="#icon-shipin"></use>
        </svg>
        <div>商品</div>
      </li>
      <li>
        <svg class="icon" aria-hidden="true">
          <use xlink:href="#icon-huiyuan"></use>
        </svg>
        <div>会员</div>
      </li>
      <li>
        <svg class="icon" aria-hidden="true">
          <use xlink:href="#icon-tongji"></use>
        </svg>
        <div>统计</div>
      </li>
      <li>
        <svg class="icon" aria-hidden="true">
          <use xlink:href="#icon-shezhi"></use>
        </svg>
        <div>设置</div>
      </li>
    </ul>
  </div>
</template>

<script>
  export default {
    name: "leftNav"
  }
</script>

<style lang="scss">
.left-nav{
  color:#FFF;font-size: 12px;height: 100%;;background: #1D8CE0;
  .icon{font-size: 24px}
  ul{}
  li{text-align: center;padding: 14px 24px;border-bottom: 1px solid #20a0ff}
}
</style>