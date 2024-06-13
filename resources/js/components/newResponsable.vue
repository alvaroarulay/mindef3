<template>
    <main class="app-content">
      <div class="app-title">
          <h1><i class="bi bi-person-add"></i> Nuevo Responsable</h1>
      </div>
      <div class="row justify-content-center">
        <div class="col-lg-12">
            <div class="card shadow-lg border-0 rounded-lg mt-12">
              <div class="card-header"><h3 class="text-center font-weight-light my-4"> Oficina: </h3></div>
              <div class="card-body">
                <div class="row">
                  <div class="mb-3 col-md-6">
                    <div class="mb-3 input-group" >
                        <div class="input-group-prepend">
                            <span class="input-group-text">
                              <label> Código:</label>
                            </span>
                        </div>
                        <input type="text" class="form-control"  placeholder="Ingrese Código de Oficina" v-model="codofic" @keyup.enter="buscarOficina()" >
                    </div>
                  </div>
                  <div class="mb-3 col-md-3 form-group">
                    <button class="btn btn-info form-control btnagregar" type="button" @click="buscarOficina()"><i class="bi bi-plus-lg" ></i> Agregar</button>
                  </div>
                  <div class="mb-3 col-md-3 form-group">
                    <button class="btn btn-success form-control btnagregar" type="button" @click="abrirModal()"><i class="bi bi-search"></i> Buscar</button>
                  </div>
                </div>
                <div class="row">
                  <div class="mb-3 col-md-12">
                    <div class="mb-3 input-group" >
                        <div class="input-group-prepend">
                            <span class="input-group-text">
                              <label> Nombre Oficina:</label>
                            </span>
                        </div>
                        <input class="form-control" type="text" v-model="nomofic" disabled>  
                    </div>
                  </div>
                </div>
                <div class="row">
                  <div class="mb-3 col-md-6">
                    <div class="mb-3 input-group" >
                        <div class="input-group-prepend">
                            <span class="input-group-text">
                              <label> Estado:</label>
                            </span>
                        </div>
                        <input class="form-control" v-if="estado==1" type="text"  value="ACTIVO" disabled>
                        <input class="form-control" v-if="estado==0" type="text"  value="INACTIVO" disabled> 
                    </div>
                  </div>
                  <div class="mb-3 col-md-6">
                    <div class="mb-3 input-group" >
                        <div class="input-group-prepend">
                            <span class="input-group-text">
                              <label> Observaciones:</label>
                            </span>
                        </div>
                        <input class="form-control" type="text" v-model="observacion" disabled>
                    </div>
                  </div>
                </div>
              </div>
            </div>
        </div>
      </div>
      <br>
      <br>
<!-- listado Responsable -->
<div class="row justify-content-center">
        <div class="col-lg-12">
            <div class="card shadow-lg border-0 rounded-lg mt-12">
              <div class="card-header">
                <div class="tile-title row">
                  <div class="col-md-10">
                      <h3 class="mb-3 text-center font-weight-light my-4">Responsables: <label v-text="total"></label></h3>
                  </div>
                  <div class="col-md-2">
                      <p class="bs-component d-grid">
                      <button type="button" @click="mostrarNuevo()" v-if="accion==1" class="btn btn-primary btn-block"><i class="bi bi-plus-square"></i>&nbsp;Nuevo</button>
                      </p>
                  </div>
                </div>
              </div>
              <div class="card-body">
                <template v-if="listado==1">
                <div class="tile-body table-responsive">
                    <table class="table table-bordered table-striped table-sm">
                        <thead>
                            <tr>
                                <th>Código</th>
                                <th>Nombre</th>
                                <th>Carnet</th>
                                <th>Expedido</th>
                                <th>Cargo</th>
                                <th>Estado</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="persona in arrayPersona" :key="persona.id">
                              <td v-text="persona.codresp"></td>
                              <td v-text="persona.nomresp"></td>
                              <td v-text="persona.ci"></td>
                              <td v-text="persona.sigla"></td>
                              <td v-text="persona.cargo"></td>
                              <td>
                                  <div v-if="persona.estado==1">
                                      <span class="me-1 badge badge-pill bg-success">Activo</span>
                                  </div>
                                  <div v-else-if="persona.estado==0">
                                      <span class="me-1 badge badge-pill bg-danger">Inactivo</span>
                                  </div>
                                  
                              </td>
                            </tr>                      
                        </tbody>
                    </table>
                    <nav>
                        <ul class="pagination">
                            <li class="page-item" v-if="pagination2.current_page > 1">
                                <a class="page-link" href="#" @click.prevent="cambiarPagina2(pagination2.current_page - 1,codofic)">Ant</a>
                            </li>
                            <li class="page-item" v-for="page in pagesNumber2" :key="page" :class="[page == isActived2 ? 'active' : '']">
                                <a class="page-link" href="#" @click.prevent="cambiarPagina2(page,codofic)" v-text="page"></a>
                            </li>
                            <li class="page-item" v-if="pagination2.current_page < pagination2.last_page">
                                <a class="page-link" href="#" @click.prevent="cambiarPagina2(pagination2.current_page + 1,codofic)">Sig</a>
                            </li>
                        </ul>
                    </nav>
                </div>
            </template>
            <!-- listado 2-->
            <template v-else-if="listado==2">
                <div class="tile-body">

                    <div class="mb-3 row">
                        <div class="col-md-6">
                            <label class="form-label">Nombre de Responsable:</label>
                            <input class="form-control" type="text" v-model="nomresp">
                        </div>
                        <div class="col-md-3">
                            <label class="form-label">Carnet de Identidad:</label>
                            <input class="form-control" type="text" v-model="ci">
                        </div>
                        <div class="col-md-3">
                            <label class="form-label">Expedido en:</label>
                            <div class="input-group">
                                <select v-model="expedido" class="form-select">
                                <option :value= 1>CHUQUISACA</option>
                                <option :value= 2>LA PAZ</option>
                                <option :value= 3>COCHABAMBA</option>
                                <option :value= 4>ORURO</option>
                                <option :value= 5>POTOSI</option>
                                <option :value= 6>TARIJA</option>
                                <option :value= 7>SANTA CRUZ</option>
                                <option :value= 8>BENI</option>
                                <option :value= 9>PANDO</option>
                                <option :value= 10>OTROS</option>
                                </select>   
                            </div>
                        </div>
                    </div>

                    <div class="mb-3 row">
                        <div class="col-md-6">
                            <label class="form-label">Cargo:</label>
                            <input class="form-control" type="text" v-model="cargo">
                        </div>
                        <div class="col-md-3">
                            <label class="form-label">Observaciones:</label>
                            <textarea class="form-control" rows="3" v-model="observ"></textarea> 
                        </div>
                         <div class="col-md-3">
                            <label class="form-label">Estado:</label>
                            <div class="input-group">
                                <select v-model="estadoresp" class="form-select">
                                    <option value='0'>INACTIVO</option>
                                    <option value='1' selected>ACTIVO</option>
                                </select>   
                            </div>
                        </div>
                    </div>
                    <div v-show="errorResponsable" class="form-group row div-error">
                            <div class="text-center text-error">
                                <div v-for="error in errorMostrarMsjResponsable" :key="error" v-text="error">

                                </div>
                            </div>
                        </div>
                </div>
                <div class="tile-footer">
                    <div class="form-group row">
                        <div class="col-md-2 mb-3">
                            <p class="bs-component d-grid">
                            <button type="button" @click="ocultarDetalle()" class="btn btn-danger btn-block"><i class="bi bi-x-square"></i>Cerrar</button>
                            </p>
                        </div>
                        <div class="col-md-2 mb-3">
                            <p class="bs-component d-grid">
                            <button type="button" class="btn btn-success btn-block" v-if="accion==1" @click="registrarResponsable()"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-floppy" viewBox="0 0 16 16">
                        <path d="M11 2H9v3h2z"/>
                        <path d="M1.5 0h11.586a1.5 1.5 0 0 1 1.06.44l1.415 1.414A1.5 1.5 0 0 1 16 2.914V14.5a1.5 1.5 0 0 1-1.5 1.5h-13A1.5 1.5 0 0 1 0 14.5v-13A1.5 1.5 0 0 1 1.5 0M1 1.5v13a.5.5 0 0 0 .5.5H2v-4.5A1.5 1.5 0 0 1 3.5 9h9a1.5 1.5 0 0 1 1.5 1.5V15h.5a.5.5 0 0 0 .5-.5V2.914a.5.5 0 0 0-.146-.353l-1.415-1.415A.5.5 0 0 0 13.086 1H13v4.5A1.5 1.5 0 0 1 11.5 7h-7A1.5 1.5 0 0 1 3 5.5V1H1.5a.5.5 0 0 0-.5.5m3 4a.5.5 0 0 0 .5.5h7a.5.5 0 0 0 .5-.5V1H4zM3 15h10v-4.5a.5.5 0 0 0-.5-.5h-9a.5.5 0 0 0-.5.5z"/>
                        </svg>Guardar</button>
                        <button type="button" class="btn btn-warning btn-block" v-if="accion==2" @click="actualizarResponsable()"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-floppy" viewBox="0 0 16 16">
                        <path d="M11 2H9v3h2z"/>
                        <path d="M1.5 0h11.586a1.5 1.5 0 0 1 1.06.44l1.415 1.414A1.5 1.5 0 0 1 16 2.914V14.5a1.5 1.5 0 0 1-1.5 1.5h-13A1.5 1.5 0 0 1 0 14.5v-13A1.5 1.5 0 0 1 1.5 0M1 1.5v13a.5.5 0 0 0 .5.5H2v-4.5A1.5 1.5 0 0 1 3.5 9h9a1.5 1.5 0 0 1 1.5 1.5V15h.5a.5.5 0 0 0 .5-.5V2.914a.5.5 0 0 0-.146-.353l-1.415-1.415A.5.5 0 0 0 13.086 1H13v4.5A1.5 1.5 0 0 1 11.5 7h-7A1.5 1.5 0 0 1 3 5.5V1H1.5a.5.5 0 0 0-.5.5m3 4a.5.5 0 0 0 .5.5h7a.5.5 0 0 0 .5-.5V1H4zM3 15h10v-4.5a.5.5 0 0 0-.5-.5h-9a.5.5 0 0 0-.5.5z"/>
                        </svg>Actualizar</button>
                        </p>
                        </div>
                    </div>
                </div>
            </template>
              </div>
              </div>
              </div>
              </div>
    <!-- Fin Listado -->


      <!----inicio modal-->
      <div class="modal fade" tabindex="-1" :class="{'mostrar' : modal}" role="dialog" aria-labelledby="myModalLabel" style="display: none;" aria-hidden="true">
            <div class="modal-dialog modal-dialog-scrollable modal-dialog-centered modal-lg" role="document">
                <div class="modal-content">
                    <div class="modal-header">
                        <h4 class="modal-title">Buscar Oficinas</h4>
                        <button type="button" class="close btn btn-sm btn-danger" @click="cerrarModal()" aria-label="Close">
                            <span aria-hidden="true">×</span>
                        </button>
                    </div>
                    <div class="modal-body">
                        <div class="form-group row">
                            <div class="col-md-6">
                                <div class="input-group mb-3">
                                    <select class="form-select col-md-3" v-model="criterio">
                                    <option value="codofic">Codigo</option>
                                    </select>
                                    <input type="text" v-model="buscar" @keyup.enter="listarOficina(1,buscar,criterio)" class="form-control" placeholder="Ingrese un Codigo">
                                    <button type="submit" @click="listarOficina(1,buscar,criterio)" class="btn btn-primary"><i class="fa fa-search"></i> Buscar</button>
                                </div>
                            </div>
                        </div>
                        <div class="table-responsive">
                            <table class="table table-bordered table-striped table-sm">
                                <thead>
                                    <tr>
                                        <th>Opciones</th>
                                        <th>Código</th>
                                        <th>Oficina</th>
                                        <th>Estado</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr v-for="oficina in arrayOficinas" :key="oficina.id">
                                        <td>
                                            <button type="button" @click="agregarDetalleModal(oficina)" class="btn btn-success btn-sm">
                                            <i class="fa fa-check"></i>
                                            </button>
                                        </td>
                                        <td v-text="oficina.codofic"></td>
                                        <td v-text="oficina.nomofic"></td>
                                        <td>
                                        <div v-if="oficina.api_estado==1">
                                            <span class="me-1 badge badge-pill bg-success">Activo</span>
                                        </div>
                                        <div v-else-if="oficina.api_estado==3">
                                            <span class="me-1 badge badge-pill bg-danger">Inactivo</span>
                                        </div>
                                        
                                    </td>
                                    </tr>                                
                                </tbody>
                            </table>
                            <nav>
                            <ul class="pagination">
                                <li class="page-item" v-if="pagination.current_page > 1">
                                    <a class="page-link" href="#" @click.prevent="cambiarPagina(pagination.current_page - 1,buscar,criterio)">Ant</a>
                                </li>
                                <li class="page-item" v-for="page in pagesNumber" :key="page" :class="[page == isActived ? 'active' : '']">
                                    <a class="page-link" href="#" @click.prevent="cambiarPagina(page,buscar,criterio)" v-text="page"></a>
                                </li>
                                <li class="page-item" v-if="pagination.current_page < pagination.last_page">
                                    <a class="page-link" href="#" @click.prevent="cambiarPagina(pagination.current_page + 1,buscar,criterio)">Sig</a>
                                </li>
                            </ul>
                        </nav>
                        </div>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" @click="cerrarModal()">Cerrar</button>
                    </div>
                </div>
                <!-- /.modal-content -->
            </div>
            <!-- /.modal-dialog -->
      </div>
    </main>
</template>
<script>
import axios from 'axios';
export default {
  data() {
    return {
        listado:1,
        accion:1,

        codofic:0,
        estado:0,
        nomofic:'',
        observacion:'',

       
        nomresp:'',
        cargo:'',
        observ:'',
        ci:'',
        total:0,
        expedido: 2,
        estadoresp : 0,

        arrayOficina:[],
        arrayOficinas:[],
        arrayPersona:[],
        errorResponsable:0,
        errorMostrarMsjResponsable:[],
        modal : 0,
        criterio:'codofic',
        buscar: '',
        pagination : {
          'total' : 0,
          'current_page' : 0,
          'per_page' : 0,
          'last_page' : 0,
          'from' : 0,
          'to' : 0,
      },
      pagination2 : {
          'total' : 0,
          'current_page' : 0,
          'per_page' : 0,
          'last_page' : 0,
          'from' : 0,
          'to' : 0,
      },
      offset : 3,
      offset2 : 3,
    }

  },
  computed:{
            isActived: function(){
                return this.pagination.current_page;
            },
            //Calcula los elementos de la paginación
            pagesNumber: function() {
                if(!this.pagination.to) {
                    return [];
                }
                
                var from = this.pagination.current_page - this.offset; 
                if(from < 1) {
                    from = 1;
                }

                var to = from + (this.offset * 2); 
                if(to >= this.pagination.last_page){
                    to = this.pagination.last_page;
                }  

                var pagesArray = [];
                while(from <= to) {
                    pagesArray.push(from);
                    from++;
                }
                return pagesArray;             

            },
            isActived2: function(){
                return this.pagination2.current_page;
            },
            //Calcula los elementos de la paginación
            pagesNumber2: function() {
                if(!this.pagination2.to) {
                    return [];
                }
                
                var from = this.pagination2.current_page - this.offset2; 
                if(from < 1) {
                    from = 1;
                }

                var to = from + (this.offset2 * 2); 
                if(to >= this.pagination2.last_page){
                    to = this.pagination2.last_page;
                }  

                var pagesArray = [];
                while(from <= to) {
                    pagesArray.push(from);
                    from++;
                }
                return pagesArray;             

            }
        },
  methods: {
    buscarOficina(){
        let me=this;
        var url= '/oficinas/buscarOficina?filtro=' + me.codofic;
        axios.get(url).then((response)=>{
            console.log(response.data.oficina);
            me.arrayOficina = response.data.oficina;
            me.codofic=me.arrayOficina.codofic;
            me.nomofic=me.arrayOficina.nomofic;
            me.observacion = me.arrayOficina.observ;
            me.estado = me.arrayOficina.api_estado;
            me.listarPersona(1,me.codofic);
        })
        .catch(function (error) {
            console.log(error);
            Swal.fire('No se Encontro Oficina','','error')
        });
    },
    listarOficina(page,buscar,criterio){
        let me=this;
        var url= '/oficinas/?page=' + page + '&buscar='+ buscar + '&criterio='+ criterio;
        axios.get(url).then( (response) =>{
            var respuesta= response.data;
            console.log(respuesta);
            me.arrayOficinas = respuesta.oficinas.data;
            me.pagination= respuesta.pagination;
        })
        .catch( (error) =>{
            console.log(error);
        });
    },
    listarPersona (page){
        let me=this;
        var url= '/responsable/listar?page=' + page + '&codofic='+ me.codofic;
        axios.get(url).then( (response) =>{
          console.log(response.data);
            var respuesta= response.data;
            me.arrayPersona = respuesta.responsables.data;
            me.pagination2= respuesta.pagination;
            me.total = respuesta.responsables.total;
        })
        .catch( (error)=> {
            console.log(error);
        });
    },
    cambiarPagina(page,buscar,criterio){
        let me = this;
        //Actualiza la página actual
        me.pagination.current_page = page;
        //Envia la petición para visualizar la data de esa página
        me.listarOficina(page,buscar,criterio);
    },
    cambiarPagina2(page,codofic){
        let me = this;
        //Actualiza la página actual
        me.pagination2.current_page = page;
        //Envia la petición para visualizar la data de esa página
        me.listarPersona(page,codofic);
    },
    cerrarModal(){
        this.modal=0;
        this.arrayOficina=[];
    }, 
    abrirModal(){  
        this.modal = 1;
        this.listarOficina(1,this.buscar,this.criterio);
    },
    agregarDetalleModal(data){
      this.codofic=data.codofic;
      this.nomofic=data.nomofic;
      this.estado=data.api_estado;
      this.observacion=data.observ;
      this.listarPersona(1,data.codofic);
      this.cerrarModal();
    },
    registrarResponsable(){
      if (this.validarResponsable()){ return;}
      axios.post('/responsable/registrar',{
            'codofic': this.codofic,
            'nomresp': this.nomresp,
            'cargo' : this.cargo,
            'ci' : this.ci,
            'expedido' : this.expedido,
            'observ' : this.observ,
            'estado' :this.estadoresp
        }).then((response)=>{
          Swal.fire(response.data.message, "", "success");
          this.codofic=0;
          this.nomresp='';
          this.cargo='';
          this.ci='';
          this.expedido= 2;
          this.observ='';
        }).catch((error)=>{
            console.log(error);
            Swal.fire(response.data.message, "", "error");
        });
    },
    validarResponsable(){
        let me = this;
        this.errorResponsable=0;
        this.errorMostrarMsjResponsable =[];
        let validText = /^[ a-zA-ZñÑáéíóúÁÉÍÓÚ]+$/;
        let validnumber = /^([0-9])*$/;
        if (me.nomresp == null || me.nomresp.length == 0 || !validText.test(me.nomresp)) me.errorMostrarMsjResponsable.push("Nombre de responsable incorrecto");
        if (me.cargo == null || me.cargo.length == 0 || !validText.test(me.cargo)) me.errorMostrarMsjResponsable.push("Cargo incorrecto");
        if (me.ci == null || me.ci.length == 0 || !validnumber.test(me.ci)) me.errorMostrarMsjResponsable.push("Carnet de Identidad incorrecto");
      
        if (this.codofic==0){ this.errorMostrarMsjResponsable.push("Seleccione una Oficina.");};
        if (this.expedido==0){ this.errorMostrarMsjResponsable.push("Seleccione un Departamento.");};
        if (this.errorMostrarMsjResponsable.length) this.errorResponsable = 1;

        return this.errorResponsable;
    },
    
    mostrarNuevo(){
        let me=this;
        if (me.codofic==0 || me.estado==0)
        {
            Swal.fire('Seleccione una Oficina u Oficina Inactiva','','error');
            me.listado=1;
        }
        else{
            me.listado=2;
        }
    },
    ocultarDetalle(){
        this.listado=1;
        this.accion=1;
        this.nomresp='';
        this.cargo='';
        this.observ='';
        this.ci=0;
        this.expedido=2;
        this.estadoresp=1;
        this.errorMostrarMsjResponsable=[];
        this.errorResponsable=[];
    },
  },
  mounted() {
   this.listarOficina(1,'','');
  }
}
</script>