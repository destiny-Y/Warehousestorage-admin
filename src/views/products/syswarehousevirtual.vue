<template>
  <div class="mod-sys__syswarehouse">
    <el-form :inline="true" :model="state.dataForm" @keyup.enter="state.getDataList()">
      <el-form-item>
        <el-button @click="state.getDataList()">{{ $t("query") }}</el-button>
      </el-form-item>
      <el-form-item>
        <el-button type="info" @click="state.exportHandle()">{{ $t("export") }}</el-button>
      </el-form-item>
      <el-form-item>
        <el-button v-if="state.hasPermission('sys:syswarehouse:save')" type="primary" @click="addOrUpdateHandle()">{{ $t("add") }}</el-button>
      </el-form-item>
      <el-form-item>
        <el-button v-if="state.hasPermission('sys:syswarehouse:delete')" type="danger" @click="state.deleteHandle()">{{ $t("deleteBatch") }}</el-button>
      </el-form-item>
    </el-form>
    <el-table v-loading="state.dataListLoading" :data="state.dataList" border @selection-change="state.dataListSelectionChangeHandle" style="width: 100%">
      <el-table-column type="selection" header-align="center" align="center" width="50"></el-table-column>
      <!-- <el-table-column prop="id" label="主键" header-align="center" align="center"></el-table-column> -->
      <el-table-column prop="name" label="仓库名称" header-align="center" align="center"></el-table-column>
      <el-table-column prop="address" label="仓库地址" header-align="center" align="center"></el-table-column>
      <el-table-column prop="userId" label="仓库负责人" header-align="center" align="center"></el-table-column>
      <!-- <el-table-column prop="type" label="仓库类型 0:虚拟仓库 1:实体仓库" header-align="center" align="center"> -->
      <el-table-column prop="type" label="仓库类型" header-align="center" align="center">
        <template v-slot="scope">
          <el-tag type="primary" v-if="scope.row.type == 1">实体仓库</el-tag>
          <el-tag type="success" v-if="scope.row.type == 0">虚拟仓库</el-tag>
        </template>
      </el-table-column>
      <!-- <el-table-column prop="delFlag" label="删除标识  0：未删除    1：删除" header-align="center" align="center"></el-table-column> -->
      <el-table-column prop="creator" label="创建者" header-align="center" align="center"></el-table-column>
      <el-table-column prop="createDate" label="创建时间" header-align="center" align="center"></el-table-column>
      <!-- <el-table-column prop="updater" label="更新者" header-align="center" align="center"></el-table-column> -->
      <el-table-column prop="updateDate" label="更新时间" header-align="center" align="center"></el-table-column>
      <el-table-column :label="$t('handle')" fixed="right" header-align="center" align="center" width="150">
        <template v-slot="scope">
          <el-button v-if="state.hasPermission('sys:syswarehouse:update')" type="primary" link @click="addOrUpdateHandle(scope.row.id)">{{ $t("update") }}</el-button>
          <el-button v-if="state.hasPermission('sys:syswarehouse:delete')" type="primary" link @click="state.deleteHandle(scope.row.id)">{{ $t("delete") }}</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination :current-page="state.page" :page-sizes="[10, 20, 50, 100]" :page-size="state.limit" :total="state.total" layout="total, sizes, prev, pager, next, jumper" @size-change="state.pageSizeChangeHandle" @current-change="state.pageCurrentChangeHandle"> </el-pagination>
    <!-- 弹窗, 新增 / 修改 -->
    <add-or-update :key="addKey" ref="addOrUpdateRef" @refreshDataList="state.getDataList"></add-or-update>
  </div>
</template>

<script lang="ts" setup>
import useView from "@/hooks/useView";
import { nextTick, reactive, ref, toRefs, watch } from "vue";
import AddOrUpdate from "./syswarehouse-add-or-update.vue";

const view = reactive({
  getDataListURL: "/sys/syswarehouse/page",
  getDataListIsPage: true,
  exportURL: "/sys/syswarehouse/export",
  deleteURL: "/sys/syswarehouse",
  deleteIsBatch: true,
  dataForm: {
  }
});

const state = reactive({ ...useView(view), ...toRefs(view) });


const addKey = ref(0);
const addOrUpdateRef = ref();
const addOrUpdateHandle = (id?: number) => {
  addKey.value++;
  nextTick(() => {
    addOrUpdateRef.value.init(id);
  });
};
</script>