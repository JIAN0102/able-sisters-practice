<template>
  <div
    id="productModal"
    ref="modal"
    class="modal fade"
    tabindex="-1"
    aria-labelledby="productModalLabel"
    aria-hidden="true"
  >
    <div
      class="modal-dialog modal-dialog-centered modal-xl"
      role="document"
    >
      <div class="modal-content border-0">
        <div class="modal-header bg-dark text-white">
          <h5
            id="productModalLabel"
            class="modal-title"
          >
            <span>新增產品</span>
          </h5>
          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="modal"
            aria-label="Close"
          />
        </div>
        <div class="modal-body">
          <div class="row">
            <div class="col-sm-4">
              <div class="mb-3">
                <label
                  for="image"
                  class="form-label"
                >輸入圖片網址</label>
                <input
                  id="image"
                  v-model="tempProduct.imageUrl"
                  type="text"
                  class="form-control"
                  placeholder="請輸入圖片連結"
                >
                <img
                  :src="tempProduct.imageUrl"
                  class="img-fluid"
                >
              </div>
              <div class="mb-3">
                <label
                  for="customFile"
                  class="form-label"
                >或 上傳圖片
                  <i class="fas fa-spinner fa-spin" />
                </label>
                <input
                  id="customFile"
                  ref="fileInput"
                  type="file"
                  class="form-control"
                  @change="uploadFile"
                >
              </div>
              <img
                class="img-fluid"
                alt=""
              >
              <!-- 延伸技巧，多圖 -->
              <div
                v-if="tempProduct.imagesUrl"
                class="mt-5"
              >
                <div
                  v-for="(image, index) in tempProduct.imagesUrl"
                  :key="index"
                  class="mb-3 input-group"
                >
                  <input
                    v-model="tempProduct.imagesUrl[index]"
                    type="url"
                    class="form-control form-control"
                    placeholder="請輸入連結"
                  >
                  <button
                    type="button"
                    class="btn btn-outline-danger"
                    @click="removeImage(index)"
                  >
                    移除
                  </button>
                </div>
                <div v-if="tempProduct.imagesUrl.length < 5">
                  <button
                    class="btn btn-outline-primary btn-sm d-block w-100"
                    @click="addImage"
                  >
                    新增圖片
                  </button>
                </div>
              </div>
            </div>
            <div class="col-sm-8">
              <div class="mb-3">
                <label
                  for="title"
                  class="form-label"
                >標題</label>
                <input
                  id="title"
                  v-model="tempProduct.title"
                  type="text"
                  class="form-control"
                  placeholder="請輸入標題"
                >
              </div>

              <div class="row gx-2">
                <div class="mb-3 col-md-6">
                  <label
                    for="category"
                    class="form-label"
                  >分類</label>
                  <input
                    id="category"
                    v-model="tempProduct.category"
                    type="text"
                    class="form-control"
                    placeholder="請輸入分類"
                  >
                </div>
                <div class="mb-3 col-md-6">
                  <label
                    for="price"
                    class="form-label"
                  >單位</label>
                  <input
                    id="unit"
                    v-model="tempProduct.unit"
                    type="text"
                    class="form-control"
                    placeholder="請輸入單位"
                  >
                </div>
              </div>

              <div class="row gx-2">
                <div class="mb-3 col-md-6">
                  <label
                    for="origin_price"
                    class="form-label"
                  >原價</label>
                  <input
                    id="origin_price"
                    v-model.number="tempProduct.origin_price"
                    type="number"
                    class="form-control"
                    placeholder="請輸入原價"
                  >
                </div>
                <div class="mb-3 col-md-6">
                  <label
                    for="price"
                    class="form-label"
                  >售價</label>
                  <input
                    id="price"
                    v-model.number="tempProduct.price"
                    type="number"
                    class="form-control"
                    placeholder="請輸入售價"
                  >
                </div>
              </div>
              <hr>

              <div class="mb-3">
                <label
                  for="description"
                  class="form-label"
                >產品描述</label>
                <textarea
                  id="description"
                  v-model="tempProduct.description"
                  type="text"
                  class="form-control"
                  placeholder="請輸入產品描述"
                />
              </div>
              <div class="mb-3">
                <label
                  for="content"
                  class="form-label"
                >說明內容</label>
                <textarea
                  id="content"
                  v-model="tempProduct.content"
                  type="text"
                  class="form-control"
                  placeholder="請輸入產品說明內容"
                />
              </div>
              <div class="mb-3">
                <div class="form-check">
                  <input
                    id="is_enabled"
                    v-model="tempProduct.is_enabled"
                    class="form-check-input"
                    type="checkbox"
                    :true-value="1"
                    :false-value="0"
                  >
                  <label
                    class="form-check-label"
                    for="is_enabled"
                  >
                    是否啟用
                  </label>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="modal-footer">
          <button
            type="button"
            class="btn btn-outline-secondary"
            data-bs-dismiss="modal"
          >
            取消
          </button>
          <button
            type="button"
            class="btn btn-primary"
            @click="$emit('update-product', tempProduct)"
          >
            確認
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import modalMixin from '@/mixins/modalMixin';

export default {
  mixins: [modalMixin],
  props: {
    product: {
      type: Object,
      default: () => ({}),
    },
  },
  data() {
    return {
      modal: {},
      tempProduct: {},
    };
  },
  watch: {
    product() {
      this.tempProduct = this.product;
    },
  },
  methods: {
    addImage() {
      this.tempProduct.imagesUrl.push('');
    },
    removeImage(index) {
      this.tempProduct.imagesUrl.splice(index, 1);
    },
    uploadFile() {
      const uploadedFile = this.$refs.fileInput.files[0];
      const formData = new FormData();
      formData.append('file-uo-upload', uploadedFile);
      const api = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/admin/upload`;
      this.$http.post(api, formData)
        .then((res) => {
          if (res.data.success) {
            this.tempProduct.imageUrl = res.data.imageUrl;
          }
        });
    },
  },
};
</script>
