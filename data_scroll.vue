
<template lang="html">
 	<div  class="scroller-component" data-role="component"
			v-on:touchmove.stop.prevent= "move"
			v-on:touchend.stop.prevent = "end"
			v-on:touchstart.stop.prevent="start"  >
			<div class="scroller-mask"  ></div>
			<div class="scroller-indicator" ></div>
			<div class="scroller-content" :style="styleObject"  >
				<div class="scroller-item" v-for = "i in dList" :value="i">{{i}}</div>
			</div>	
	</div>
</template>
<script lang="js">
		'use strict';
		 module.exports ={
		         data:function(){
		    	 return {
				    	styleObject:{
							transform:'translate(0,-68px)'			            	             	                    
			            },
			             startY:"0",
			             endY:'0',
			             moveY:"0",
			             Y:"0",
			             isMoving:false,  
			             d:"0",		       
			             cellstep:34/37.5,
			             dList:this.datalist,
			             unit:"rem",
			             remcell:37.5,
			             maxmove:0,
			             inilength:3,
			             focusData:"",
			             aY:0,
			             dType:this.datatype,
			             dTime:this.defaultdata
		    	 }
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
			  methods:{
				  move:move,
				  end:end,
				  start:start,
				  refresh:refresh
			  },
			  mounted:function(){	
				  var vm=this;
				  refresh(vm);
			  },
			  watch:{
				  datalist:cdatalist,
				  defaultdata:cdefaultdata,
				  focusData:cfocusData
			  }
		}
		
		 
		 function refresh(vm){
			 var cellstep=vm.cellstep,
			 inilength=vm.inilength;
			 vm.maxmove=(vm.dList.length-inilength)*cellstep;
			 vm.focusData=vm.dList[inilength-1];
			 if(vm.dList.length<=inilength){
				 vm.focusData=vm.dList[vm.dList.length-1];
			 }
			 if(vm.dTime!=undefined&&vm.dTime!=""){
				 vm.focusData=vm.dTime;	
			 }
			 var focuslength=inilength-1-vm.dList.indexOf(vm.focusData);
			 vm.styleObject.transform="translate(0,"+focuslength*cellstep+vm.unit+")";
			 vm.Y=focuslength*cellstep;			 
		 }		 
		 
		  function cdatalist(){
			  this.dList=this.datalist;
			  var vm=this;
			  refresh(vm);			 
		  }
		  
		  
		  function cdefaultdata(){
			  this.dTime=this.defaultdata;
			  var vm=this;
			  refresh(vm);
		  }
		  
		  function cfocusData(){
			  var vm=this;
			  var cellstep=vm.cellstep,
				 inilength=vm.inilength;
			    var focuslength=inilength-1-vm.dList.indexOf(vm.focusData);
				vm.styleObject.transform="translate(0,"+focuslength*cellstep+vm.unit+")";
				vm.Y=focuslength*cellstep;
		  }		  

		  
		  function end(e){
			var vm=this,
			cellstep=vm.cellstep,
			inilength=vm.inilength,
			remcell=vm.remcell;
			var mT = 0.3,unit=vm.unit;
            vm.moveY = e.changedTouches[0].clientY/remcell-vm.startY ;
            vm.Y = parseFloat(vm.moveY) + parseFloat(vm.Y);          
            vm.d= Math.round(vm.Y/cellstep);
            vm.Y = vm.d*cellstep;
            if(vm.Y>cellstep*(inilength-1)){
            	vm.Y=cellstep*(inilength-1);
		  	}
            if(vm.Y<=-vm.maxmove){
            	vm.Y=-vm.maxmove;
            }
            console.log("vm.Y"+vm.Y);
            vm.styleObject.transform  = "translate(0,"+vm.Y+unit+")";
            var dList=vm.dList;
            vm.d=vm.Y/cellstep;
            vm.focusData=dList[inilength-1-vm.d];
            this.$emit('focus-data',{dType:vm.dType,focusData:vm.focusData});
            vm.timeOut = setTimeout(function () {
                vm.isMoving = false;
            },mT)
		  }
		  
		  
		  function move(e){
			var vm=this,
			cellstep=vm.cellstep,
		    inilength=vm.inilength,
		    remcell=vm.remcell;
	        var unit=vm.unit;		              
	        vm.moveY= e.changedTouches[0].clientY/remcell -vm.startY;
	        vm.aY = parseFloat(vm.moveY) + parseFloat(vm.Y);
	        console.log("move.vm.Y"+vm.Y);
	        if(vm.aY>cellstep*(inilength-1)){
            	vm.aY=cellstep*(inilength-1);
		  	}
	        
            if(vm.aY<=-vm.maxmove){
            	vm.aY=-vm.maxmove;
            }
	        vm.styleObject.transform  = "translate(0,"+vm.aY+unit+")";	
		  }
		 
		  
		  function start(e){
			  var vm=this,
			  cellstep=vm.cellstep,
			  inilength=vm.inilength,
			  remcell=vm.remcell;
	          if(vm.isMoving){
	            return false;
	          }
	          vm.startY = e.changedTouches[0].clientY/remcell;
	          console.log("vm.startY------------------------------------"+vm.startY);
		  }
</script>