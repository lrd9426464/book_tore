<template>
   <div class="shop">
      <!-- <div @click="click">123465</div> -->
      <!-- <mt-badge type="success">10</mt-badge> -->
      <div id="order_head">
         <span>目前有</span>
         <mt-badge class="badge" size="small" color="transparent">{{sum}}</mt-badge>
         <span v-if="this.sum==1 || this.sum==0">种商品</span>
         <span v-else>种不同种类的商品</span>
      </div>
      <div id="content">
         <div id="order_content" v-for="(item,i) in $store.state.shoppingCar" :key="i">
            <div id="order_msg">
               <div>
                  <input type="checkbox" @click="oneSelect">
               </div>
               <div>
                  <img src="http://view.jqueryfuns.com/2020/6/17/389352992b566007a4a27e492e992458/images/book.jpg" alt="">
                  <div>
                     <p>{{item.title}}</p>
                     <mt-button :data-num="i" @click.native="minus" size="small">-</mt-button>
                        <span >{{item.count}}</span>
                     <mt-button :data-num="i" @click.native="add" size="small">+</mt-button>
                  </div>
               </div>
            </div>
            <div id="order_price">
               <div>
                  <img src="../assets/shopping/del.png" alt="">
                  <span :data-num="i" @click="del">删除</span>
               </div>
               <div class="aaa">订单总价：¥<span>{{item.price*item.count}}</span></div>
            </div>
         </div>
         <div id="nothing" v-show="isNothing">
            <p>
               购物车竟然是空的
            </p>
            <p>
               快去买点什么吧！
            </p>
         </div>
      </div>
      <div id="pay">
         <input size="40" type="checkbox" id="p1" @click="allSelect"><label for="p1">全选</label>
         <span>合计：¥{{allPrice}}</span>
         <router-link to @click.native="submit">提交订单({{num}})</router-link>
      </div>
      <my-footer></my-footer>
   </div>
</template>

<script>
export default {
   data(){
      return {
         sum:0,
         num:0,
         isNothing:false,
         // allPrice:0
      }
   },
   methods:{
      minus(e){
         // let num=e.target.dataset.num;
         let num=e.target.dataset.num;
         // console.log(this.$store.state.shoppingCar[num1].count)
         // let span=document.getElementById(num);
         // let total=document.getElementById(`total${num}`);
         // console.log(span)
         // console.log(num)
         // let count=e.target.nextElementSibling.innerHTML
         // console.log(count)
         if(this.$store.state.shoppingCar[num].count>1){
            this.$store.commit("minus_count",num);
            // span.innerHTML--
            // window.localStorage.setItem(`count${num}`,span.innerHTML);
            // console.log(window.localStorage.getItem(`count${num}`))
            // e.target.nextElementSibling.innerHTML=window.localStorage.getItem(`count${num}`);
            // window.localStorage.setItem(`price${num}`,e.target.nextElementSibling.innerHTML*this.commodity[num1].price);
            // total.innerHTML=window.localStorage.getItem(`price${num}`);
            // this.allPrice-=Number(this.commodity[e.target.dataset.num].price);
            // this.commodity[num].n=e.target.nextElementSibling.innerHTML;
            // this.aa=this.commodity;
            // console.log(total)
         }
      },
      add(e){
         // let num=e.target.dataset.num;
         let num=e.target.dataset.num;
         this.$store.commit("add_count",num);
         // let span=document.getElementById(num);
         // console.log(span)
         // let total=document.getElementById(`total${num}`);
         // span.innerHTML++;
         // total.innerHTML=e.target.previousElementSibling.innerHTML*this.commodity[num1].price;
         // window.localStorage.setItem(`price${num}`,total.innerHTML);
         // window.localStorage.setItem(`count${num}`,span.innerHTML);
         // this.allPrice+=Number(this.commodity[e.target.dataset.num].price);
      },
      del(e){
         let num=e.target.dataset.num;
         let inputs=document.querySelectorAll("#content input");
         let p1=document.getElementById("p1");
         inputs.forEach((elem,i)=>{
            elem.checked=false;
            // console.log(i)
            this.$store.commit("nochecked",i);
            p1.checked=false;
         })
         if(this.$store.state.shoppingCar.length==0){
            this.isNothing=true;
            p1.checked=false;
            // this.allPrice-=Number(delprice);
         }
         this.$store.commit("del_commodity",num);
         this.sum=this.$store.state.shoppingCar.length;
         this.num=0;
      },
      allSelect(){
         // let span=document.querySelectorAll(".aaa span");
         // console.log(span)
         let inputs=document.querySelectorAll("#content input");
         let p1=document.getElementById("p1");
         if(p1.checked==true){
            inputs.forEach((elem,i)=>{
               if(elem.checked!=true){
                  elem.checked=true;
                  this.$store.commit("checked",i);
               }
            })
            this.num=inputs.length;
         }else{
            this.num=0;
            inputs.forEach((element,i) => {
               if(element.checked==true){
                  element.checked=false;
                  this.$store.commit("nochecked",i);
               }
            });
         }
      },
      oneSelect(){
         this.commodity=[];
         let input=document.querySelectorAll("#content input");
         let p1=document.getElementById("p1")
         let arr=[];
         input.forEach((elem,i)=>{
            if(elem.checked==true){
               // console.log(i)
               this.$store.commit("checked",i);
               // console.log(this.$store.state.shoppingCar[i])
               this.commodity.push(this.$store.state.shoppingCar[i]);
               arr.push(elem);
            }else{
               this.$store.commit("nochecked",i);
            }
         })
         // console.log(arr.length);
         this.num=arr.length;
         if(arr.length==input.length){
            p1.checked=true;
         }else{
            p1.checked=false;
         }
      },
      submit(){
         let noclear=[];
         let commodity=[];
         let input=document.querySelectorAll("#content input");
         input.forEach((elem,i)=>{
            if(elem.checked==true){
               console.log("1")
               commodity.push(this.$store.state.shoppingCar[i])
               console.log(commodity)
            }else{
               noclear.push(this.$store.state.shoppingCar[i]);
            }
         })
         this.$store.commit("pay_shopping",noclear);
         if(this.num!=0){
            this.$store.commit("pay",commodity);
            // console.log(this.$store.state.pay_commodity)
            this.$router.push(`/pay`);
         }
      }
   },
   mounted(){
      let inputs=document.querySelectorAll("#content input");
      inputs.forEach((elem,i)=>{
         elem.checked=false;
         this.$store.commit("nochecked",i);
      })
      if(this.$store.state.shoppingCar==null){
         this.num=0;
      }else{
         let n=0;
         let input=document.querySelectorAll("#content input");
         let p1=document.getElementById("p1")
         this.sum=this.$store.state.shoppingCar.length;
         if(this.$store.state.shoppingCar.length!==0){
            this.$store.state.shoppingCar.forEach((elem,i)=>{
               if(elem.selected==1){
                  n++;
                  input[i].checked=true;
               }
            })
         }
         
         if(n==input.length && n!==0){
            p1.checked=true;
         }else{
            p1.checked=false;
         }
      }
      // console.log(this.sum)
      if(this.sum==0){
         this.isNothing=true;
      }
   },
   computed:{
      allPrice(){
         // console.log("123");
         let num=0;
         if(this.$store.state.shoppingCar!=0){
            this.$store.state.shoppingCar.forEach(elem=>{
               if(elem.selected==1){
                  num+=Number(elem.count)*Number(elem.price);
               }
            })
         }
         return num;
      }
   }
}
</script>

<style scoped>
   .shop #pay{
      height: 46px;
      width: 100%;
      line-height: 46px;
      position: fixed;
      bottom: 60px;
      background: white;
   }
   .shop #pay span,
   .shop #pay input{
      margin-left: 45px;
   }
   .shop #pay a{
      text-decoration: none;
      color: white;
      background-color: red;
      padding: 0px 20px ;
      border-radius: 20px;
      box-sizing: border-box;
      position: absolute;
      right: 10px;
   }
   .shop #order_head{
      background-color: #26a2ff;
      padding-left: 10px;
   }
   .shop #order_head span{
      line-height: 50px;
   }
   .shop #content{
      margin-bottom: 110px;
   }
   .shop #order_content{
      margin: 10px 0;
      /* background-color: aqua; */
   }
   .shop #nothing{
      width: 100%;
      margin: 250px 0;
      text-align: center;
   }
   .shop #nothing p:first-child{
      font-size: 20px;
      color: #ccc;
   }
   .shop #nothing p:last-child{
      margin-top: 10px;
      font-size: 15px;
      color: #bbb;
   }
   .shop #order_msg{
      display: flex;
      height: 100px;
   }
   .shop #order_msg>div:first-child{
      flex: 1;
      text-align: center;
      line-height: 100px;
   }
   .shop #order_msg>div:last-child{
      display: flex;
      padding: 10px;
      flex: 4;
   }
   .shop #order_msg>div:last-child img{
      height: 80px;
      margin-right: 20px;
   }
   .shop #order_msg>div:last-child>div>p{
      margin-bottom: 30px;
   }
   .shop #order_msg>div:last-child>div>span{
      margin: 0 8px;
   }
   .shop #order_price{
      height: 30px;
      display: flex;
      line-height: 30px;
   }
   .shop #order_price div:first-child{
      flex: 2;
      margin-left: 30px;
   }
   .shop #order_price div:last-child{
      flex: 3;
      margin-left: 120px;
   }
   .shop #order_price div:last-child>span{
      color: red;
   }
</style>