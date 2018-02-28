<template lang="html">
		<div class="scroller-component" data-role="component"
			v-on:touchmove.stop.prevent= "move"
			v-on:touchend.stop.prevent = "end"
			v-on:touchstart.stop.prevent="start"  >
			<div class="scroller-mask"></div>
			<div class="scroller-indicator"></div>
			<div class="day-wheel">
				<div  v-for="(item,index) in dList"    class="wheel-div"    :style="'transform: rotate3d(1, 0, 0,'+ (inilength-index)*deg+'deg) translate3d(0px, 0px, '+radii+unit+')'"  v-text="item">									        	
				</div>
			</div>	
		</div>
</template>

<script lang="js">
	module.exports ={
	    data:function(){
	    	return {
				dList:["北京市","河南省","天津市","河北省","湖北省","湖南省","山西省"],
				deg:35,
				radii:62/37.5,
				unit:"rem",
				eunitl:62/(60*37.5),
				remcell:37.5,
		        startY:"0",
	            moveY:"0",
	            Y:"0", 
	            d:"0",
	            aY:0,
	            inilength:4,
				focusData:"",
	            dListset:this.datalist,
	            dType:this.datatype,
	            dTime:this.defaultdata,
	            circle:0
	    	}
	    },
	    methods:{
			move:move,
			end:end,
			start:start,
			getdList:getdList,
			refresh:refresh
	    },
	    props:{
		  	 datatype:{
				 type:String
		     },
			datalist:{
		  	    type:Array
		     },
		     defaultdata:{
		  	   type:String
		     }
		},
		mounted:function(){
			var vm=this;
			if(vm.dListset){
				if(vm.dListset.length>0){
					refresh(vm);
				}
			}
		},
		watch:{
			datalist:cdatalist,
			defaultdata:cdefaultdata
		}
	}
	
	function refresh(vm){
		   var eunitl=vm.eunitl,inilength=vm.inilength,dListset=vm.dListset,deg=vm.deg;
			if(dListset.length>inilength){
				vm.focusData=dListset[inilength];
			}else{
				vm.focusData=dListset[dListset.length-1];
			}
			if(vm.dTime!=undefined&&vm.dTime!=""){
				vm.focusData=vm.dTime;
			}
			var focuslength=dListset.indexOf(vm.focusData)-inilength+vm.circle*vm.dListset.length;
			vm.Y=-focuslength*eunitl*deg;
			vm.getdList(focuslength);
	}		 

	 function cdatalist(){
		  var vm=this;
		  vm.dListset=this.datalist;
		  refresh(vm); 		 
	 }

	 function cdefaultdata(){
		  var vm=this;
		  vm.dTime=this.defaultdata;
		  refresh(vm);
	 }
	 	
	function end(e){
	 	var vm=this,
		mT = 0.3,unit=vm.unit,dListset=vm.dListset,inilength=vm.inilength,remcell=vm.remcell;
	    vm.moveY = e.changedTouches[0].clientY/remcell - vm.startY;
	    vm.Y = parseFloat(vm.moveY) + parseFloat(vm.Y);
	    vm.circle=parseInt(vm.Y/(vm.dListset.length*vm.eunitl*vm.deg));
	    var indexL=Math.ceil((vm.Y%(vm.dListset.length*vm.eunitl*vm.deg))/(vm.eunitl*vm.deg));	    
	    indexL=(dListset.length-indexL)%dListset.length;
		vm.getdList(indexL);
		vm.focusData=dListset[(indexL+dListset.length+inilength)%dListset.length];
		this.$emit('focus-data',{dType:vm.dType,focusData:vm.focusData});
	    vm.timeOut=setTimeout(function () {
	        vm.isMoving = false;
	    },mT) 
	}
	  
	  function move(e){
	 	var vm=this,
	    unit=vm.unit,remcell=vm.remcell;
	    vm.moveY= e.changedTouches[0].clientY/remcell -  vm.startY;
	    vm.aY = parseFloat(vm.moveY) + parseFloat(vm.Y);
		var indexL=Math.ceil((vm.aY%(vm.dListset.length*vm.eunitl*vm.deg))/(vm.eunitl*vm.deg));
		indexL=(vm.dListset.length-indexL)%vm.dListset.length;
		vm.getdList(indexL);
	  }
	 
	  
	  function start(e){
	 	  var vm=this,remcell=vm.remcell;
	      vm.startY = e.changedTouches[0].clientY/remcell;
	  }

	  function getdList(index){

		  var vm=this;
		  var dListset=vm.dListset;
		  var d=new Array();
		  var inilength=vm.inilength;			  
		  var dListsetlen=dListset.length;
		  if(index<0){
		  	index=(dListsetlen+index)%dListsetlen;
		  }
		  var maxlength=index+2*inilength+1;	
		  if(dListsetlen-1<maxlength&&index>=0&&dListsetlen>2*inilength+1){	  
			  var dList=[],dList1=[];			  
			  dList=dListset.slice(index,dListsetlen);
			  dList1=dList.concat(dListset.slice(0,maxlength-dListsetlen));
			  vm.dList=dList1;
			  return false;
		  }else if(dListsetlen-1>=maxlength&&index>=0&&dListsetlen>2*inilength+1){
			  vm.dList=dListset.slice(index,maxlength);
			  return false;
		  }else{  
			  vm.dList=dListset;
			  return false;
		  }
	  }

</script>

