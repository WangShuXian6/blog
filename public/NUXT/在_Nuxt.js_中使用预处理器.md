>在 Nuxt.js 中使用预处理器

##### Less

```bash
npm install -D less less-loader
```
>nuxt.config.js 添加

```javascript
export default {
  build: {
    // You cannot use ~/ or @/ here since it's a Webpack plugin
    styleResources: {
      less: './assets/*.less'
    }
  }
}
```
>pages/index.vue

```vue
<template>
  <!--<section>-->
  <!--<h1 class="header">Nuxt TypeScript Starter</h1>-->
  <!--<div class="cards">-->
  <!--<Card v-for="person in people" :key="person.id" :person="person"></Card>-->
  <!--</div>-->
  <!--</section>-->
  <!---->
  <!---->
  <el-container style="height: 500px; border: 1px solid #eee">

    <el-aside width="200px" style="background-color: rgb(238, 241, 246)">

      <el-menu :default-openeds="['1', '3']">

        <el-submenu index="1">

          <template slot="title"><i class="el-icon-message"></i>Navigator One</template>

          <el-menu-item-group>

            <template slot="title">Group 1</template>

            <el-menu-item index="1-1">Option 1</el-menu-item>

            <el-menu-item index="1-2">Option 2</el-menu-item>

          </el-menu-item-group>

          <el-menu-item-group title="Group 2">

            <el-menu-item index="1-3">Option 3</el-menu-item>

          </el-menu-item-group>

          <el-submenu index="1-4">

            <template slot="title">Option4</template>

            <el-menu-item index="1-4-1">Option 4-1</el-menu-item>

          </el-submenu>

        </el-submenu>

        <el-submenu index="2">

          <template slot="title"><i class="el-icon-menu"></i>Navigator Two</template>

          <el-menu-item-group>

            <template slot="title">Group 1</template>

            <el-menu-item index="2-1">Option 1</el-menu-item>

            <el-menu-item index="2-2">Option 2</el-menu-item>

          </el-menu-item-group>

          <el-menu-item-group title="Group 2">

            <el-menu-item index="2-3">Option 3</el-menu-item>

          </el-menu-item-group>

          <el-submenu index="2-4">

            <template slot="title">Option 4</template>

            <el-menu-item index="2-4-1">Option 4-1</el-menu-item>

          </el-submenu>

        </el-submenu>

        <el-submenu index="3">

          <template slot="title"><i class="el-icon-setting"></i>Navigator Three</template>

          <el-menu-item-group>

            <template slot="title">Group 1</template>

            <el-menu-item index="3-1">Option 1</el-menu-item>

            <el-menu-item index="3-2">Option 2</el-menu-item>

          </el-menu-item-group>

          <el-menu-item-group title="Group 2">

            <el-menu-item index="3-3">Option 3</el-menu-item>

          </el-menu-item-group>

          <el-submenu index="3-4">

            <template slot="title">Option 4</template>

            <el-menu-item index="3-4-1">Option 4-1</el-menu-item>

          </el-submenu>

        </el-submenu>

      </el-menu>

    </el-aside>


    <el-container>

      <el-header style="text-align: right; font-size: 12px">

        <el-dropdown>

          <i class="el-icon-setting" style="margin-right: 15px"></i>

          <el-dropdown-menu slot="dropdown">

            <el-dropdown-item>View</el-dropdown-item>

            <el-dropdown-item>Add</el-dropdown-item>

            <el-dropdown-item>Delete</el-dropdown-item>

          </el-dropdown-menu>

        </el-dropdown>

        <span>Tom</span>

      </el-header>


      <el-main>

        <el-table :data="tableData">

          <el-table-column prop="date" label="Date" width="140">

          </el-table-column>

          <el-table-column prop="name" label="Name" width="120">

          </el-table-column>

          <el-table-column prop="address" label="Address">

          </el-table-column>

        </el-table>

      </el-main>

    </el-container>

  </el-container>
</template>

<script lang="ts">
  import {
    Component,
    Vue
  } from "nuxt-property-decorator"
  import {State} from "vuex-class"
  import Card from "~/components/Card.vue"

  const item = {
    date: '2016-05-02',
    name: 'Tom',
    address: 'No. 189, Grove St, Los Angeles'
  }

  @Component({
    components: {
      Card
    }
  })
  export default class extends Vue {
    @State people
    tableData = Array(20).fill(item)
  }
</script>
<style scoped lang="less">
  .header {
    font-family: "Segoe UI", Tahoma, Geneva, Verdana,
    sans-serif;
  }

  .cards {
    display: flex;
    flex-wrap: wrap;
  }

  .el-header {
    background-color: #B3C0D1;
    color: #333;
    line-height: 60px;
  }

  .el-aside {
    color: #333;
  }
</style>
```