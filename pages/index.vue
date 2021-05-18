<template>
  <div>
    <logo/>
    <el-header> 
        Bienvenido, gracias por participar en este estudio. A continuación, realice la busqueda de las siguientes palabras y al final de cada busqueda, escriba comos se sintió con los resultados. 
    </el-header>
    <div class="sb-container">
      <div class="gcse-searchbox"></div>
    </div>
    <div class="gcse-searchresults"></div>
    <div id="element" class="cardcss" >
      <form id="formulario" @submit.prevent="submitHandler" >
        <label for="sentiment">Por favor escriba como se siente</label>
        <input class="box" type="text" required v-model="sentiment">
       <div class="submit">
         <button class="button">
           Enviar
         </button>
       </div>
      </form>
    </div>      
  </div>
</template>

<script lang="ts">
import { Vue, Component } from 'vue-property-decorator'

@Component
export default class SearchComponent extends Vue {
  private userId?: string
  
  data(){
    return{
      sentiment:''
      
    }
  }
 submitHandler (event:Event){
  console.log(event)
  console.log(this.sentiment)
  this.addMeasure('sentiment',this.sentiment)
 

 }
  async motionHandler(event: DeviceMotionEvent) {
    console.log(event.acceleration)
    this.addMeasure('movement',{x:event.acceleration?.x,y:event.acceleration?.y,z:event.acceleration?.z})
    
  }

 async addMeasure (key: string, data: any) {
   console.log(key,data)
    await this.$fire.firestore.collection('measures').doc(this.userId ?? '0')
      .collection(key)
      .add({
        timestamp: new Date(),
        ...data,
      });
  }
  
  orientationHandler(event: DeviceOrientationEvent){
    this.addMeasure('orientation',{aplha:event.alpha,beta:event.beta,gamma:event.gamma});
  }  
  
  mouseHandler(event: MouseEvent) {
    console.log(event)
    this.addMeasure('mouse',{screenX:event.screenX, screenY:event.screenY})
  }
  
  touchHandler(event:TouchEvent){
    
    console.log(event)
    this.addMeasure('touch',{screenX:event.changedTouches[0].screenX,screenY:event.changedTouches[0].screenY})
   
    
  }
  lightHandler(event: DeviceLightEvent) {
    console.log(event)
  }

  async beforeMount() {
    const authentication = await this.$fire.auth.signInAnonymously()
    this.userId = authentication.user?.uid
  }

  async mounted() {
    window.addEventListener('touchmove', this.touchHandler.bind(this))
    window.addEventListener('mousemove', this.mouseHandler.bind(this))
    window.addEventListener(
      'deviceorientation',
      this.orientationHandler.bind(this)
    )
    window.addEventListener('devicemotion', this.motionHandler.bind(this))
    window.addEventListener('devicelight', this.lightHandler.bind(this))
    
  }

  unmounted() {
    window.removeEventListener('touchmove', this.touchHandler.bind(this))
    window.removeEventListener('mousemove', this.mouseHandler.bind(this))
    window.removeEventListener(
      'deviceorientation',
      this.orientationHandler.bind(this)
    )
    window.removeEventListener('devicemotion', this.motionHandler.bind(this))
    window.removeEventListener('devicelight', this.lightHandler.bind(this))
  }
}
</script>
<style>
 .el-header{
     text-align: center;
     margin-top: 5px;
     margin-bottom: 50px;
 }
 .box{
   margin-top: 5px;
   margin-bottom: 5px;
   padding: 5px;
 }
 .cardcss{
  text-align: center;
 }
 .submit{
   text-align: center;
 }
 .button{
   margin-top: 5px;
   margin-bottom: 5px;
   padding: 5px;
   border-radius: 10%;
   background-color:lightskyblue
 }
</style>
