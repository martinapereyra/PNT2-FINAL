<template>
  <section class="src-componentes-formulario-av">
    <div class="jumbotron">
      <h2>Conversor a Dólares</h2>
      <br>
      <form >
        <div>
          <label for="monto">Ingrese monto $ </label>
          <input 
            type="number" 
            id="monto" 
            class="form-control" 
            v-model.trim="formData.monto" 
            name="monto"
          >
       </div>
    
        <br>
        <div >
          <label for="number">Valor en dolar $ </label>
          <input  v-if="!formData.check"
            type="text" 
            id="dolar" 
            class="form-control" 
            v-model.trim="formData.dolar" 
            name="dolar"
          >
          <input  v-else
            type="text" 
            id="dolar" 
            class="form-control" 
            v-model.trim="this.valorDolarActual" 
            name="dolar"
          >
            <label v-if="formData.check" style="margin-left:1rem" for="checkbox">- Actualización: <span style="font-weight: bold">{{this.hora}}</span>  </label>
            <label v-else style="margin-left:1rem" for="checkbox">- Actualización:</label>
            <input name="checkbox" type="checkbox" v-model ="formData.check">
        </div>

        <br>
      </form>    

      <p v-if="!formData.check">Valor Convertido en USD : {{ convertirDolar }} </p>
      <p v-else >Valor Convertido en USD : {{ convertirDolarApi}}  </p>
    <br>
    <hr>
     <div>
        <h3>Respuestas : </h3>
        <ul>
            <li>1:c</li>
            <li>2:a</li>
            <li>3:c</li>
            <li>4:a</li>
            <li>5:b</li>
        </ul>
     </div>

    </div>
  </section>

</template>

<script>

  export default  {
    name: 'src-componentes-formulario-av',
    props: [],
    mounted () {
        this.intervalo()
    },
    data () {
      return {
        formState : {},
        formData: this.getInitialData(),
        urlApi: "https://www.dolarsi.com/api/api.php?type=valoresprincipales",
        valorDolarActual: "",
        hora: "",
      }
    },
    methods: {
      getInitialData() {
        return {
          monto: null,
          dolar: null,
          check: false,
        }
    },
    
    async getDolarApi(){
        try{
        let {data:res} = await this.axios.get(this.urlApi)
        this.valorDolarActual = res[1].casa.venta;
      }
      catch(error) { console.error('Error en getPelicula por Id', error.message) } 

    },

    intervalo(){
        setInterval(() => {
            this.getDolarApi()
            this.ObtenerHora()
        }, 2000);
    },

    ObtenerHora(){
         this.hora = new Date().toLocaleString();
    }


    },
    computed: {
        convertirDolar(){
            let total = this.formData.dolar * this.formData.monto;
            return total;
        },
        
        convertirDolarApi(){
           let total = parseInt(this.valorDolarActual) * this.formData.monto
           return total
        },


    }
}


</script>

<style scoped lang="css">

  .jumbotron {
    color: black;
  }

  hr {
    background-color: #bbb;
  }

  pre {
    color: white;
  }
</style>
