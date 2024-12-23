<script setup lang="ts">
import { isArray } from 'wot-design-uni/components/common/util'
import type { FormInstance, FormRules } from 'wot-design-uni/components/wd-form/types'

const model = reactive<{
  personName: string
  phone: string
  address: string[]
  province: string
  city: string
  area: string
  detailAddress: string
}>({
  personName: '',
  phone: '',
  address: [],
  province: '',
  city: '',
  area: '',
  detailAddress: '',
})

const rules: FormRules = {
  personName: [
    { required: true, message: '请填写收货人' },
    { required: true, min: 2, max: 10, message: '收货人姓名长度在2到10个字符之间' },
  ],
  phone: [
    { required: true, message: '请填写手机号' },
    { required: true, pattern: /^1[3-9]\d{9}$/, message: '手机号格式不正确' },
  ],
  address: [
    {
      required: true,
      message: '请选择地址',
      validator: async (value) => {
        if (isArray(value) && value.length) {
          return Promise.resolve()
        }
        else {
          return Promise.reject('请选择地址')
        }
      },
    },
  ],
  detailAddress: [
    { required: true, message: '请填写详细地址' },
    { required: true, min: 5, max: 120, message: '详细地址长度在5到120个字符之间' },
  ],
}

const form = ref<FormInstance>()

function handleSubmit() {
  form
    .value!.validate()
    .then(({ valid, errors }) => {
      console.log(valid)
      console.log(errors)
    })
    .catch((error) => {
      console.log(error, 'error')
    })
}
</script>

<template>
  <view class="px-3 py-20 text-center">
    <view>
      <wd-form ref="form" error-type="toast" :model="model" :rules="rules">
        <wd-cell-group border>
          <wd-input
            v-model="model.personName" label="收货人" required label-width="68px" prop="personName"
            placeholder="请填写收货人"
          />
          <wd-input v-model="model.phone" label="手机号" required label-width="68px" prop="phone" placeholder="请填写手机号" />
          <wd-col-picker
            v-model="model.address" label-width="68px" required label="所在地址" placeholder="请选择地址"
            prop="address" :columns="area" :column-change="columnChange" @confirm="handleConfirm"
          />
          <wd-cell title="详细地址" required vertical>
            <wd-textarea
              v-model="model.detailAddress" placeholder="请填写详细地址" prop="detailAddress" :maxlength="120"
              clearable show-word-limit
            />
          </wd-cell>
        </wd-cell-group>
        <view class="m-a w-80%">
          <wd-button type="primary" block @click="handleSubmit">
            保存
          </wd-button>
        </view>
      </wd-form>
    </view>
  </view>
</template>

<style></style>

<route type="home" lang="json">
{
  "layout": "tabbar",
  "name": "home",
  "style": {
    "navigationBarTitleText": "home"
  }
}
</route>
