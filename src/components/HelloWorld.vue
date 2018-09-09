<template>
  <div class="menu">
    <div class="set" v-show="showSet">
      <div class="set-title">设置</div>
      <div class="layout-set">
          <div class="layout-set-title">布局设置</div>
          <div class="layout-set-content">
            <span>菜单最大宽度：</span>
            <Input v-model="layout.list"></Input>
            <span>菜品宽度：</span>
            <Input v-model="layout.item"></Input>
            <Button@click="clearLocalStorage" class="add-button" type="success">清空缓存菜品信息</Button>
          </div>
      </div>
      <div class="title-set">
          <div class="label">菜单标题：</div>
          <Input v-model="title" />
      </div>
      <div class="content-set">
          <div class="set-add">
            <Button class="add-button" @click="addList(0)" type="primary">添加分类</Button>
          </div>
          <div class="set-item" v-for="(data, idx) of menuList" :key="idx">
            <div class="food-title">
                <div class="label">菜品类名</div>
                <Input v-model="data.title" />
            </div>
            <div class="food-list">
                <div>菜品列表</div>
                <Row>
                    <Col class="col" :span="8">菜名</Col>
                    <Col class="col" :span="4">单价</Col>
                    <Col class="col" :span="4">单位</Col>
                    <Col class="col" :span="4"></Col>
                    <Col class="col" :span="4"></Col>
                </Row>
                <div v-for="(food, index) of data.menus" :key="index" class="food-item">        
                    <Row>
                        <Col class="col" :span="8">
                            <Input v-model="food.title"></Input>
                        </Col>
                        <Col class="col" :span="4">
                            <Input v-model="food.price"></Input>
                        </Col>
                        <Col class="col" :span="4">
                            <Input v-model="food.unit"></Input>
                        </Col>
                        <Col class="col" :span="4">
                            <Button type="success" @click="addItem(data.menus, idx + 1)">添加</Button>
                        </Col>
                        <Col class="col" :span="4">
                            <Button type="warning" v-if="data.menus && data.menus.length > 1" @click="deleteItem(data.menus, index)">删除</Button>
                        </Col>
                    </Row>
                </div>
            </div>
            <Button @click="deleteList(idx)" type="warning" class="add-button">删除分类</Button>
            <Button @click="addList(idx+1)" class="add-button" type="primary">添加分类</Button>
          </div>
      </div>
    </div>
    <div
        class="show"
        :style="{
            maxWidth: Number(layout.list) ? layout.list + 'px' : '100%'
        }">
      <Button class="hide" type="primary" v-if="showSet" @click="showSet=!showSet">隐藏设置界面</Button>
      <Button class="hide" type="primary" @click="showSet=!showSet" v-else>显示设置界面</Button>
      <div class="logo">
          <img src="./logo.jpg" alt="logo" class="logo-image">
          <div class="show-title">{{title}}</div>
          <div class="show-slogon">欢迎光临</div>
      </div>
      <div class="show-content">
          <div v-for="(data, idx) of menuList" class="menu-type" :key="idx">
              <div class="type-title">
                  <div class="content">{{data.title}}</div>
                  <div class="line"></div>
              </div>
              <div class="type-items">
                  <div
                    v-for="(item, index) of data.menus"
                    :key="index"
                    :style="{
                        minWidth: Number(layout.item) ? layout.item + 'px' : 'auto'
                    }"
                    class="type-item">
                      <div class="item-title">
                        <Checkbox>{{item.title}}</Checkbox>
                      </div>
                      <div class="item-price">
                          ￥{{item.price}}/{{item.unit}}
                      </div>
                  </div>
              </div>
          </div>
      </div>
      <div class="footer">
          <img src="" alt="">
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      title: '烤撸台',
      slogon: '祝您用餐愉快',
      showSet: true,
      layout: {
          list: '',
          item: ''
      },
      menuList: [{
        title: '招牌菜',
        menus: [{
            title: '竹筒烧肉',
            price: '35',
            unit: '筒'
        },
        {
            title: '竹筒米饭',
            price: '25',
            unit: '筒',
        },
        {
            title: '爆炒花甲',
            price: '30',
            unit: '份'
        },
        {
            title: '啤酒小龙虾(小份)',
            price: '58',
            unit: '份'            
        },
        {
            title: '啤酒小龙虾(大份)',
            price: '78',
            unit: '份'            
        },
        {
            title: '海鲜拼盘',
            price: '58',
            unit: '份'
        }]
      },
      {
          title: '私房菜',
          menus: [{
              title: '包烧鱼',
              price: '30',
              unit: '条'
          },
          {
              title: '包烧茄子',
              price: '20',
              unit: '包'
          },
          {
              title: '包烧牛肉',
              price: '35',
              unit: '份'
          },
          {
              title: '包烧猪脑',
              price: '10',
              unit: '个'
          }]
      },
      {
          title: '串串',
          menus: [{
              title: '鱿鱼',
              price: '5',
              unit: '串'
          },
          {
              title: '羊肉串',
              price: '1.5',
              unit: '串'
          },
          {
              title: '青豆',
              price: '1',
              unit: '串'
          }]
      },
      {
          title: '肉类',
          menus: [{
              title: '鸡翅中',
              price: '4',
              unit: '个'
          },
          {
              title: '鸡翅尖',
              price: '1',
              unit: '个'
          },
          {
              title: '秘制五花肉',
              price: '25',
              unit: '份'
          },
          {
              title: '猪皮',
              price: '20',
              unit: '份'
          },
          {
              title: '鸡脚筋',
              price: '20',
              unit: '份'
          },
          {
              title: '牛板筋',
              price: '25',
              unit: '份'
          },
          {
              title: '秘制牛肉片',
              price: '35',
              unit: '份'
          },
          {
              title: '腰花',
              price: '25',
              unit: '份'
          }]
      },
      {
          title: '素菜',
          menus: [{
              title: '土豆',
              price: '10',
              unit: '份'
          },
          {
              title: '小瓜',
              price: '10',
              unit: '份'
          },
          {
              title: '韭菜',
              price: '10',
              unit: '份'
          },
          {
              title: '人工菌',
              price: '10',
              unit: '份'
          },
          {
              title: '香菇',
              price: '10',
              unit: '份'
          },
          {
              title: '金针菇',
              price: '13',
              unit: '份'
          },
          {
              title: '糯米藕',
              price: '15',
              unit: '份'
          }]
      }]
    }
  },
  mounted () {
      let title = window.localStorage.getItem('title')
      if (title) {
          this.title = title
      }
      let menuList = window.localStorage.getItem('menuList')
      if (menuList) {
          let list = JSON.parse(menuList)
          this.menuList = list
      }
  },
  methods: {
      addList (idx) {
          let data = {
              title: '',
              menus: []
          }
          this.menuList.splice(idx, 0, data)
          this.addItem(data.menus)
      },
      deleteList (idx) {
          this.menuList.splice(idx, 1)
      },
      addItem (list, idx) {
          let data = {
              title: '',
              price: '',
              unit: '份'
          }
          if (idx) {
              list.splice(idx, 0, data)
          } else {
              list.push(data)
          }
      },
      deleteItem (list, idx) {
          list.splice(idx, 1)
      },
      clearLocalStorage () {
          window.localStorage.removeItem('title')
          window.localStorage.removeItem('menuList')          
          window.location.reload()
      }
  },
  watch: {
      menuList () {
          let list = JSON.stringify(this.menuList)
          window.localStorage.setItem('menuList', list)
      },
      title () {
          window.localStorage.setItem('title', this.title)
      }
  }
}
</script>

<style lang="stylus" scoped>
.menu
    display flex
    position fixed
    top 0
    bottom 0
    left 0
    right 0
    .set
        width 50%
        min-width 300px
        max-width 500px
        text-align left
        overflow-y auto
        .set-title
            text-align center
            font-size 1rem
            padding 1rem
        .title-set, .content-set, .layout-set
            margin 4px
            padding 0.5rem
            box-shadow 0 0 1px 1px #6190E8
            border-radius 4px
        .content-set
            .set-item
                box-shadow 0 0 1px 1px #6190E8
                border-radius 4px
                padding 2px
                margin 0.5rem 0
    .show
        flex auto
        display flex
        flex-direction column
        align-items center
        overflow-y auto
        position relative
        .logo
            padding 1rem
            display flex
            align-items center
            justify-content flex-start
            padding-left 15%
            width 100%
            .logo-image
                width 6rem
                height 6rem
            .show-title
                font-size 1rem
                width 1rem
            .show-slogon
                font-size 2rem
                position relative
                left 10rem
        .hide
            width 100%
            opacity 0
            position absolute
            z-index 1
        .show-content
            flex auto
            background #FAFAFA
            display flex
            flex-direction column
            .menu-type
                box-shadow 0 0 1px 1px #9B9B9B
                margin 0.5rem
                border-radius 4px
                padding 0.5rem
                .type-title
                    display flex
                    align-items center
                    .line
                        margin-left 0.5rem
                        flex auto
                        height 1px
                        background #9B9B9B
                .type-items
                    display flex
                    flex-wrap wrap
                    .type-item
                        width 8rem
                        margin-right 1rem
                        display flex
                        align-items center
                        .item-title
                            flex auto
                            text-align left
                            margin-right 0.5rem
.col, .col-left
    display flex
    align-items center
    justify-content center
    margin-bottom 0.5rem
    padding-right 0.5rem
.col-left
    justify-content flex-start
.add-button
    width 100%
    margin 4px 0
</style>
