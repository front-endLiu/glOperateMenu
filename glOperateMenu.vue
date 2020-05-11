<!--表格操作按钮组件-->
<script>
export default {
  props: {
    operateGroup: {
      // {权限role, 是否展示isShow, 文本text, 属性attrs, 事件on}
      type: Array,
      default: () => []
    },
    maxNum: {
      // 操作最多展示个数
      type: Number,
      default: 3
    }
  },
  render(h) {
    let newMenus = this.operateGroup.filter(
      item =>
        item.isShow &&
        this.$store.state.userInfo.permissions.includes(item.role)
    ); // 过滤掉不显示的 和 没有权限的
    let hFun = [];
    if (newMenus.length <= this.maxNum) {
      // 如果菜单组个数少于等于最大个数
      newMenus.forEach((item, index) => {
        hFun.push(
          h(
            "el-link",
            {
              class: {
                "operate-menu": newMenus.length - 1 !== index
              },
              props: {
                underline: false
              },
              attrs: {
                ...item.attrs
              },
              on: {
                ...item.on
              }
            },
            [item.text]
          )
        );
      });
    } else {
      // 菜单数超过最大个数
      let dropdownMenu = [];
      newMenus.forEach((item, index) => {
        if (index < this.maxNum) {
          hFun.push(
            h(
              "el-link",
              {
                class: {
                  "operate-menu": true
                },
                props: {
                  underline: false
                },
                attrs: {
                  ...item.attrs
                },
                on: {
                  ...item.on
                }
              },
              [item.text]
            )
          );
        } else {
          dropdownMenu.push(
            h(
              "el-dropdown-item",
              {
                class: {},
                props: {
                  underline: false
                },
                attrs: {
                  ...item.attrs
                },
                nativeOn: {
                  ...item.on
                }
              },
              [item.text]
            )
          );
        } // #if else
      });

      hFun.push(
        h("el-dropdown", {}, [
          h(
            "el-link",
            {
              props: {
                underline: false
              },
              attrs: {
                type: "primary"
              }
            },
            [
              h(
                "i",
                {
                  class: {
                    "el-icon-more": true
                  }
                },
                []
              )
            ]
          ),
          h(
            "el-dropdown-menu",
            {
              slot: "dropdown"
            },
            [...dropdownMenu]
          )
        ])
      );
    }
    return h("div", {}, [...hFun]);
  }
};
</script>
<style lang="less" scoped>
.operate-menu {
  margin-right: 8px;
  &:after {
    width: 1px;
    background-color: #d9d9d9;
    height: 14px;
    content: "";
    margin-left: 8px;
  }
}
/deep/ .el-dropdown-menu__item:focus,
.el-dropdown-menu__item:not(.is-disabled):hover {
  color: #a25f29;
}
</style>