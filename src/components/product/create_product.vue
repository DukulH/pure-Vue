<template>
    <div>
        <Header headerName="Product Create"></Header>
        <div
    class="m-4"
    v-loading="loading"
    :active="show_loader"
    element-loading-text="Loading..."
  >
    <div class="d-flex justify-content-between">
        <div class="ms-2 mb-3">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item>Products</el-breadcrumb-item>
                <el-breadcrumb-item>Add Product</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="me-2 mb-3">
            <el-button-group>
          <el-button @click="goBackFunction" type="info">Back</el-button>
          <el-button @click="productListFunction" type="info" plain>
            <i class=""></i> Product List
          </el-button>
        </el-button-group>
        </div>
    </div>
    <el-card class="box-card">
        <div class="w-75">
            <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="120px" class="demo-ruleForm">
                <el-form-item label="Product name" prop="product_name">
                    <el-input v-model="ruleForm.product_name"></el-input>
                </el-form-item>

                <div class="row">
                    <div class="col-6">
                        <el-form-item label="In Stock" prop="product_stock">
                            <el-input type="number" pattern="[0-9]+" min="0" v-model="ruleForm.product_stock"></el-input>
                        </el-form-item>
                    </div>
                    <div class="col-6">
                        <el-form-item label="Qty( gm )" prop="product_quantity">
                            <el-input v-model="ruleForm.product_quantity"></el-input>
                        </el-form-item>
                    </div>
                </div>
                <div class="row">
                    <div class="col-6">
                        <el-form-item label="Sales Price" prop="unit_price">
                            <el-input type="number" min="0" v-model="ruleForm.unit_price"></el-input>
                        </el-form-item>
                    </div>
                    <div class="col-6">
                        <el-form-item label="Purchase Price" prop="purchase_price">
                            <el-input type="number" min="0" v-model="ruleForm.purchase_price"></el-input>
                        </el-form-item>
                    </div>
                </div>

                <el-form-item label="Product Details" prop="desc">
                    <el-input type="textarea" v-model="ruleForm.desc"></el-input>
                </el-form-item>
                <el-form-item>
                    <el-button type="info" style="background-color:#010101;" @click="submitForm('ruleForm')">Create</el-button>
                    <el-button @click="resetForm('ruleForm')">Reset</el-button>
                </el-form-item>
            </el-form>
        </div>
    </el-card>
</div>
    </div>

</template>

<script>
import axios from "axios";
import Header from "../header/Header.vue";
import authHeader from "../authentication/authHeader"

export default {
    name: "CreateProduct",
    components:{
        Header,
    },
    data() {
        return {
            show_loader:false,
            ruleForm: {
                product_name: "",
                product_stock: "",
                product_quantity: "",
                unit_price: "",
                purchase_price: "",
                desc: "",
            },
            rules: {
                product_name: [{
                    required: true,
                    message: "Please input product name",
                    trigger: "change",
                }, ],
                product_stock: [{
                    required: true,
                    message: "Please input stock quantity",
                    trigger: "change",
                }, ],
                product_quantity: [{
                    required: true,
                    message: "Please input product quantity in (gm)",
                    trigger: "change",
                }, ],
                unit_price: [{
                    required: true,
                    message: "Please input sales price",
                    trigger: "change",
                }, ],
                purchase_price: [{
                    required: true,
                    message: "Please input purchase price",
                    trigger: "change",
                }, ],

                desc: [{
                    message: "Please input product description",
                    trigger: "change",
                }, ],
            },
        };
    },
    methods: {
        goto_list() {
            this.$router.push("/products");
        },
        goBackFunction() {
            this.$router.go(-1);
        },
        productListFunction() {
            this.$router.push("/products");
        },
        submitForm(formName) {
            this.show_loader = true
            this.$refs[formName].validate((valid) => {
                console.log(this.ruleForm);
                if (valid) {
                    axios
                        .post("/backend/createProduct/", this.ruleForm ,{headers: authHeader()})
                        .then((response) => {
                            this.show_loader=false
                            if (response) {
                                this.$notify({
                                    type: "success",
                                    title: "Success",
                                    verticalAlign: "top",
                                    message: "Product added Successfully",
                                });
                                this.goto_list();
                            }
                        });
                } else {
                    console.log("error submit!!");
                    return false;
                }
            });
        },
        resetForm(formName) {
            this.$refs[formName].resetFields();
        },
    },
};
</script> 

<style>
input[type="number"]::-webkit-inner-spin-button,
input[type="number"]::-webkit-outer-spin-button {
    -webkit-appearance: none;
}

input[type="number"] {
    -moz-appearance: textfield;
}
</style>
