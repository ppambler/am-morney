<template>
  <Layout class-prefix="layout">
    <NumberPad :value.sync="record.amount" @submit="saveRecord" />
    <Tabs :data-source="recordTypeList" :value.sync="record.type" />
    <div class="createdAt">
      <FormItem
        field-name="日期"
        type="date"
        placeholder="在这里了输入日期"
        :value.sync="record.createdAt"
      >
      </FormItem>
    </div>
    <div class="notes">
      <FormItem
        field-name="备注"
        placeholder="在这里输入备注"
        :value.sync="record.notes"
      />
    </div>
    <Tags :value.sync="record.tags" />
  </Layout>
</template>

<script lang="ts">
import Vue from "vue";
import NumberPad from "@/components/Money/NumberPad.vue";
import FormItem from "@/components/Money/FormItem.vue";
import Tags from "@/components/Money/Tags.vue";
import Tabs from "@/components/Tabs.vue";
import { Component } from "vue-property-decorator";
import recordTypeList from "@/constants/recordTypeList";
import day from "dayjs";

@Component({
  components: {
    NumberPad,
    Tabs,
    FormItem,
    Tags,
  },
})
export default class Money extends Vue {
  get recordList() {
    return this.$store.state.recordList;
  }
  recordTypeList = recordTypeList;
  record: RecordItem = {
    tags: [],
    notes: "",
    type: "-",
    amount: 0,
    createdAt: day(new Date()).format("YYYY-MM-DD"),
  };
  created() {
    this.$store.commit("fetchRecords");
  }

  saveRecord() {
    this.$store.commit("createRecord", this.record);
    this.record.notes = "";
    this.record.tags = [];
  }
}
</script>

<style lang="scss" scoped>
::v-deep .layout-content {
  display: flex;
  flex-direction: column-reverse;
}
.notes {
  padding: 10px 0 0 0;
}
.createdAt {
  padding: 0 0 10px 0;
}
</style>

