<template>
    <el-container>
        <el-header>
            <el-menu style="margin-left: 10%"
                     :default-active="activeIndex2"
                     class="el-menu-demo"
                     mode="horizontal"
                     @select="handleSelect"
                     background-color="#545c64"
                     text-color="#fff"
                     active-text-color="#ffd04b">
                <el-menu-item index="1">处理中心</el-menu-item>
                <el-menu-item index="2">消息中心</el-menu-item>
            </el-menu>

            <div class="head-portrait">
                <el-dropdown @command="clickButton" placement="bottom-start">
                    <div>
                        <div class="head-cont">
                            <img v-if="usrInfo.headImg" :src="usrInfo.headImg" alt=""/>
                            <span v-if="!usrInfo.headImg">{{ firstName }}</span>
                        </div>
                        <p class="user_name" style="color: #fff">{{ usrInfo.name }}</p>
                    </div>
                    <template #dropdown>
                        <el-dropdown-menu slot="dropdown">
                            <el-dropdown-item command="userinfo">个人中心</el-dropdown-item>
                            <el-dropdown-item command="logout">退出登录</el-dropdown-item>
                        </el-dropdown-menu>
                    </template>
                </el-dropdown>
            </div>
        </el-header>
        <el-container style="width: 100%">
            <el-aside width="210px">
                <el-menu v-for="(menu, i) in menuData" :key="i" background-color="#545c64" text-color="#fff"
                         active-text-color="#ffd04b">
                    <el-submenu :index="i + 1 + ''">
                        <template #title><i :class="menu.icon"></i>{{ menu.title }}</template>
                        <el-menu-item-group v-for="(childrenMenu, childrenIndex) in menu.children" :key="childrenIndex">
                            <el-menu-item :index="i - childrenIndex + ''" @click="addTab(childrenMenu)">
                                {{ childrenMenu.title }}
                            </el-menu-item>
                        </el-menu-item-group>
                    </el-submenu>
                </el-menu>
            </el-aside>

            <el-container style="width: 90%">
                <el-main>
                    <el-breadcrumb separator-class="el-icon-arrow-right">
                        <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
                        <el-breadcrumb-item>活动管理</el-breadcrumb-item>
                        <el-breadcrumb-item>活动列表</el-breadcrumb-item>
                        <el-breadcrumb-item>活动详情</el-breadcrumb-item>
                    </el-breadcrumb>
                    <el-tabs v-model="editableTabsValue" type="card" closable @tab-remove="removeTab">
                        <el-tab-pane
                            v-for="(item, index) in editableTabs"
                            :key="item.name"
                            :label="item.title"
                            :name="item.name">
                            {{ item.content }}
                        </el-tab-pane>
                    </el-tabs>
                </el-main>
            </el-container>
        </el-container>
    </el-container>
</template>

<script>
export default {
    name: "HelloWorld",
    data() {
        const item = {
            date: "2016-05-02",
            name: "王小虎",
            address: "上海市普陀区金沙江路 1518 弄",
        };

        return {
            tableData: Array(20).fill(item),
            indexData: {
                title: "首页",
                id: "shouye",
                url: "https://element-plus.gitee.io/#/zh-CN/component/icon",
            },
            menuData: [{
                title: "首页",
                id: "shouye",
                url: "https://element-plus.gitee.io/#/zh-CN/component/icon",
            }, {
                title: "导航一",
                id: "daohang1",
                icon: "el-icon-thumb",
                children: [
                    {
                        title: "子标题1",
                        id: "zibiaoti1",
                        url: "https://element-plus.gitee.io/#/zh-CN/component/icon",
                    },
                    {
                        title: "子标题2",
                        id: "zibiaoti2",
                        url: "https://cn.vuejs.org/v2/guide/routing.html",
                    },
                    {
                        title: "子标题3",
                        id: "zibiaoti3",
                        url: "https://www.cnblogs.com/wangyfax/p/10073159.html",
                    },
                ],
            },
            ],
            usrInfo: {
                name: "屈小舒",
            },
            activeIndex: '1',
            activeIndex2: '1',
            editableTabsValue: '2',
            editableTabs: []
        };
    },
    methods: {
        addTab(targetName) {
            let targetOpenCount = this.editableTabs.filter(table => table.name === targetName.id).length;
            console.log(targetOpenCount)
            if (targetOpenCount === 0) {
                this.editableTabs.push({
                    title: targetName.title,
                    name: targetName.id,
                    content: targetName.url
                });
            }
            this.editableTabsValue = targetName.id;
        }, removeTab(targetName) {
            let tabs = this.editableTabs;
            let activeName = this.editableTabsValue;
            if (activeName === targetName.id) {
                tabs.forEach((tab, index) => {
                    if (tab.name === targetName.name) {
                        let nextTab = tabs[index + 1] || tabs[index - 1];
                        if (nextTab) {
                            activeName = nextTab.name;
                        }
                    }
                });
            }

            this.editableTabsValue = activeName;
            this.editableTabs = tabs.filter(tab => tab.name !== targetName);
        }, clickButton(button) {
            if (button === "userinfo") {
                this.$message('打开用户信息');
            } else if (button === 'logout') {
                this.$message('用户退出');
            }
        }, handleSelect(key, keyPath) {
            console.log(key, keyPath);
        }
    }
    ,
    computed: {
        firstName() {
            return this.usrInfo.name.substr(0, 1)
        }
    }
}
;
</script>

<style>
li {
    list-style: none;
}

.el-header {
    background-color: #545c64;
    /*background-color: #2e9afe;*/
    color: #333;
    height: 60px;
    width: 100%;
    display: flex;
    justify-content: space-between;
}

.el-row {
    height: 60px;
}

.el-aside {
    background-color: #545c64;
    color: #333;
}

.el-main {
    background-color: #e9eef3;
    color: #333;
    width: 100%;
    height: 855px;
}

.logo {
    /* height: 100%; */
    display: flex;
}

.top-nav {
    padding: 0 20px;
}

.logo img {
    height: 60px;
}

.head-portrait {
    /*padding-right: 10px;*/
}

.head-cont {
    display: inline-block;
    border-radius: 50%;
    background-color: aliceblue;
    width: 50px;
    height: 50px;
    margin-top: 5px;
    text-align: center;
    font-size: 25px;
    margin-right: 10px;
}

.head-cont span {
    line-height: 50px;
}

.head-cont img {
    width: 50px;
    height: 50px;
    border-radius: 50%;
}

.user_name {
    display: inline-block;
    vertical-align: top;
    line-height: 36px;
}

.el-dropdown {
    vertical-align: top;
    height: 100%;
}

.el-dropdown + .el-dropdown {
    margin-left: 3px;
}

.el-icon-arrow-down {
    font-size: 6px;
}
</style>
