<template>
  <div
    class="menu"
    :class="{
      front: showFront && !showSet && !showBack && !showSign,
      back: showBack && !showSet && !showFront && !showSign
    }">
    <div class="choose">
      <div @click="showSet=!showSet">{{ showSet ? '隐藏设置' : '显示设置' }}</div>
      <div @click="showFront=!showFront">{{ showFront ? '隐藏正面' : '显示正面'}}</div>
      <div @click="showBack=!showBack">{{ showBack ? '隐藏反面' : '显示反面'}}</div>
      <div @click="showSign=!showSign">{{ showSign ? '隐藏招牌' : '显示招牌'}}</div>
    </div>
    <div class="set" v-show="showSet">
      <div class="set-title">设置</div>
      <div class="layout-set">
        <div class="layout-set-title">布局设置</div>
        <div class="layout-set-content">
        <span>菜品宽度：</span>
        <InputNumber :min="1" v-model="itemWidth"></InputNumber>
        <span> 每行数量：</span>
        <InputNumber :min="2" v-model="itemNums"></InputNumber>
        <span> 间距:</span>
        <InputNumber :min="1" v-model="itemDistance"></InputNumber>
        <Button@click="clearLocalStorage" class="add-button" type="success">清空缓存菜品信息</Button>
        </div>
      </div>
      <div class="title-set">
        <div class="label">菜单标题：</div>
        <Input v-model="title" />
        <div class="label">底部文字</div>
        <Input v-model="footerText" type="textarea"></Input>
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
        <div class="hot">
          <Checkbox v-model="data.hot">热门</Checkbox>
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
    <div class="show" v-show="showFront">
      <div class="bakgroud"></div>
      <div class="logo">
        <div class="logo-image" @click="showSet=!showSet"></div>
        <div class="show-title">{{title}}</div>
        <div class="show-header">菜单</div>
      </div>
      <div class="show-content">
        <div
          v-for="(data, idx) of menuList"
          class="menu-type"
          :style="{
            maxWidth: lineWidth + 'rem'
          }"
          :key="idx">
          <Divider class="divider" orientation="left">
            <div class="type-title">
              <span class="title-content">
                {{data.title}}
              </span>
              <Icon
                class="hot"
                v-if="data.hot"
                type="ios-flame">
              </Icon>
            </div>
          </Divider>
          <div class="type-items">
            <div
            v-for="(item, index) of data.menus"
            :key="index"
            :style="{
              width: itemWidth + 'rem',
              marginRight: itemDistance + 'rem'
            }"
            class="type-item">
              <div class="item-title">
                <div class="checkbox"></div>
                {{item.title}}
              </div>
              <div class="item-price">
                ￥{{item.price}}/{{item.unit}}
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="tips">
        <div v-html="footerText"></div>
      </div>
      <div class="footer">
        欢迎光临<span class="name">{{title}}</span>，祝您用餐愉快！
      </div>
    </div>
    <div class="back" v-show="showBack">
      <div class="back-content" @dblclick="showFront=!showFront">
        {{title}}
      </div>
    </div>
    <div class="signboarg-page" v-show="showSign">
      <div class="large-title">
        <span>烤</span>
        <span>撸</span>
        <span>台</span>
        <span>操</span>
        <span>作</span>
        <span>间</span>
      </div>
      <div class="small-title">
        <span>铁</span>
        <span>板</span>
        <span>烧</span>
        <span>烤</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      title: '烤撸台',
      showSet: false,
      showFront: false,
      showBack: false,
      showSign: true,
      itemWidth: 16,
      itemNums: 2,
      itemDistance: 1,
      menuList: [
        {
          title: '招牌菜',
          hot: true,
          menus: [
            {
              title: '竹筒米饭',
              price: '25',
              unit: '筒',
            },
            {
              title: '竹筒烧肉',
              price: '35',
              unit: '筒'
            },
            {
              title: '爆炒花甲',
              price: '30',
              unit: '份'
            },
            {
              title: '海鲜拼盘',
              price: '58',
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
            }
          ]
        },
        {
          title: '私房菜',
          hot: true,
          menus: [
            {
              title: '包烧猪脑',
              price: '10',
              unit: '个'
            },
            {
              title: '包烧茄子',
              price: '20',
              unit: '包'
            },
            {
              title: '包烧鱼',
              price: '30',
              unit: '条'
            },
            {
              title: '包烧牛肉',
              price: '35',
              unit: '份'
            }
          ]
        },
        {
          title: '海鲜类',
          hot: false,
          menus: [
            {
              title: '扇贝',
              price: '6',
              unit: '个'
            },
            {
              title: '生蚝',
              price: '6',
              unit: '个'
            },
            {
              title: '鱿鱼',
              price: '15',
              unit: '份'
            },
            {
              title: '鲜虾',
              price: '25',
              unit: '份'
            }
          ]
        },
        {
          title: '肉类',
          hot: false,
          menus: [
            {
              title: '火腿肠',
              price: '15',
              unit: '份'
            },
            {
              title: '鸡翅尖',
              price: '15',
              unit: '份'
            },
            {
              title: '鸡翅中',
              price: '20',
              unit: '份'
            },
            {
              title: '鸡翘',
              price: '20',
              unit: '份'
            },
            {
              title: '鸡脚筋',
              price: '20',
              unit: '份'
            },
            {
              title: '鸡胗',
              price: '20',
              unit: '份'
            },
            {
              title: '猪皮',
              price: '20',
              unit: '份'
            },
            {
              title: '腰花',
              price: '25',
              unit: '份'
            },
            {
              title: '秘制五花肉',
              price: '25',
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
            }
          ]
        },
        {
          title: '串串',
          hot: false,
          menus: [
            {
              title: '羊肉串',
              price: '1.5',
              unit: '串'
            },
            {
              title: '骨肉相连',
              price: '2',
              unit: '串'
            }
          ]
        },
        {
          title: '素菜',
          hot: false,
          menus: [
            {
              title: '青豆',
              price: '10',
              unit: '份'
            },
            {
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
              title: '包浆豆腐',
              price: '13',
              unit: '份'
            },
            {
              title: '糯米藕',
              price: '15',
              unit: '份'
            }
          ]
        },
        {
          title: '酒水',
          hot: false,
          menus: [
            {
              title: '杨梅泡酒',
              price: '30',
              unit: '壶'
            },
            {
              title: '腾冲土锅酒',
              price: '45',
              unit: '壶'
            },
            {
              title: '精致红啤(小扎)',
              price: '50',
              unit: '扎'
            },
            {
              title: '精致红啤(大扎)',
              price: '99',
              unit: '扎'
            }
          ]
        },
        {
          title: '蘸水',
          hot: false,
          menus: [
            {
              title: '腌菜膏(腾冲风味)',
              price: '2',
              unit: '份'
            },
            {
              title: '蒜香乳腐味',
              price: '2',
              unit: '份'
            },
            {
              title: '辣椒粉',
              price: '2',
              unit: '份'
            }
          ]
        }
      ],
      footerText: ''
    }
  },
  computed: {
    lineWidth () {
      return (this.itemWidth + this.itemDistance) * this.itemNums + 4
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
    let itemWidth = window.localStorage.getItem('itemWidth')
    if (itemWidth) {
      this.itemWidth = Number(itemWidth)
    }
    let itemNums = window.localStorage.getItem('itemNums')
    if (itemNums) {
      this.itemNums = Number(itemNums)
    }
    let itemDistance = window.localStorage.getItem('itemDistance')
    if (itemDistance) {
      this.itemDistance = Number(itemDistance)
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
    },
    itemWidth () {
      if (!this.itemWidth) return
      window.localStorage.setItem('itemWidth', this.itemWidth)
    },
    itemNums () {
      if (!this.itemNums) return
      window.localStorage.setItem('itemNums', this.itemNums)
    },
    itemDistance () {
      if (!this.itemDistance) return
      window.localStorage.setItem('itemDistance', this.itemDistance)
    }
  }
}
</script>

<style lang="stylus" scoped>

@font-face
  font-family 'simple font'
  src url(/static/font/font2.ttf)

@font-face
  font-family 'menu font'
  src url(/static/font/font3.ttf)

@font-face
  font-family 'logo font'
  src url(/static/font/font1.ttf)

@font-face
  font-family 'xing kai'
  src url(/static/font/xingkai.TTF)

.menu
  display flex
  position fixed
  top 0
  bottom 0
  left 0
  right 0
  justify-content center
  align-items center
  &.front
    background top left/100% 100% url(/static/img/background2.jpg)
  &.back
    background top left/100% 100% url(/static/img/background3.jpg)
  .choose
    position absolute
    background white
    color black
    left 0
    top 0
    padding 1rem
    opacity 0
    user-select none
    background #F7F7F7
    box-shadow 0 0 1px 1px #6190E8
    border-radius 4px
    &:hover
      opacity 1
    div
      cursor pointer
      margin 2px
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
    display flex
    flex-direction column
    align-items center
    overflow-y auto
    position relative
    .bakgroud
      z-index -1
      position absolute
      left 0
      right 0
      top 0
      bottom 0
    .logo
      margin 1rem auto
      display flex
      align-items center
      justify-content flex-start
      width 100%
      font-family 'logo font'
      .logo-image
        margin-left 15%
        background top/100% url('/static/img/logo.jpg')
        width 6rem
        height 6rem
        border-radius 3rem
      .show-title
        font-size 1.5rem
        color #FF4949
        width 1rem
        text-shadow 3px -3px 5px black
      .show-header
        font-size 4rem
        position relative
        left 4rem
        text-shadow 2px 2px 7px white
    .show-content
      flex auto
      display flex
      flex-direction column
      justify-content space-between
      color black
      text-shadow 1px -1px 2px #F7F7F7 
      .menu-type
        box-shadow 0 0 1px 1px #483b0b, 0 0 3px 1px #6d8416 inset
        margin 0.5rem 2rem
        border-radius 4px
        padding 0 1rem
        padding-bottom 1rem
        .divider
          margin 0
          .type-title
            display flex
            align-items center
            font-size 18px
            .title-content
              color #FF3939
              font-weight 400
              text-shadow 3px -3px 1px black
              font-family 'logo font'
            .hot
              color #FFB41B
              padding-bottom 2px
        .type-items
          display flex
          flex-wrap wrap
          font-weight 500
          font-size 16px
          .type-item
            display flex
            align-items center
            .item-title
              flex auto
              text-align left
              display flex
              align-items center
              .checkbox
                border 1px solid #9B9B9B
                border-radius 2px
                background #FAFBFC
                min-width 16px
                min-height 16px
                display inline-block
                margin auto 4px
    .tips
      width 100%
      text-align left
      div
        margin 0 2rem
        font-size 20px
        font-weight 500
        font-family 'logo font'
        color black
        text-shadow 1px -1px 2px #F7F7F7
    .footer
      margin-bottom 1rem
      font-size 1.5rem
      font-weight 1000
      text-shadow 2px 2px 1px #C5D9E8
      font-style italic
      letter-spacing 2px
      margin 1rem auto
      .name
        font-family 'logo font'
        color #FF4949
        font-weight 500
        text-shadow 2px 2px 5px black
        font-size 2rem
        letter-spacing -2px
  .back
    font-family 'logo font'
    font-size 8rem
    color #FF4949
    text-shadow 6px 6px 15px black
    display flex
    align-items center
    justify-content center
    .back-content
      width 8rem
  .signboarg-page
    width 120rem
    height 16rem
    border 1px solid red
    display flex
    flex-direction column
    justify-content space-around
    font-family 'xing kai'
    .large-title
      height 10rem
      font-size 10rem
      display flex
      justify-content space-around
      color #FF2121
      align-items center
      padding 0 10rem
    .small-title
      display flex
      align-items center
      width 30%
      position relative
      left 70rem
      justify-content space-around
      height 6rem
      font-size 6rem
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
