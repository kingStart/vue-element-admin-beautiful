<template>
  <div class="app-container">
    <TableExhibitionQuery :headers="headers" />
    <el-row>
      <el-col :span="6">
        <TableExhibitionHeader :headers.sync="headers" />
      </el-col>
      <el-col :span="18">
        <TableExhibitionBody :headers="headers" :list="list" />
      </el-col>
    </el-row>
  </div>
</template>

<script>
import TableExhibitionBody from "./TableExhibitionBody";
import TableExhibitionQuery from "./TableExhibitionQuery";
import TableExhibitionHeader from "./TableExhibitionHeader";
import { genTableSnippet } from "./snippetTable.js";

export default {
  components: {
    TableExhibitionBody,
    TableExhibitionHeader,
    TableExhibitionQuery,
  },
  props: {
    tableData: {
      type: Object,
      default: () => {
        return {};
      },
    },
  },
  data() {
    return {
      list: [],
      code: "",
      headers: [],
      query: {
        limit: 20,
        cursor: 1,
      },
      total: 0,
    };
  },
  watch: {
    tableData: {
      handler(val) {
        this.list = val.data && val.data.slice(0, 3);
        if (this.list) {
          this.headers = Object.keys(this.list[0]).map((item) => {
            // opt 为需要对 字段的操作，''空就不操作，'time' 处理时间，'template' 将字段放在template 里面，方便后续操作
            return {
              value: this.list[0][item],
              key: item,
              label: item,
              show: true,
              opt: "",
              query: false,
            };
          });
        }
      },
      immediate: true,
    },
    headers: {
      handler(val) {
        this.code = genTableSnippet(val, this.getTableAPI);
        this.$store.commit("SET_SRC_TABLE_CODE", this.code);
        /* window.localStorage.removeItem('vue-element-table-gen-code');
                          window.localStorage.setItem('vue-element-table-gen-code', this.code); */
      },
      deep: true,
    },
  },
  created() {},
  methods: {
    editdata() {},
    test(val) {},
  },
};
</script>
