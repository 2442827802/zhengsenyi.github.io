<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>rain</title>
		<style type="text/css">
			body,html{
				margin: 0px;
				height: 100%;
			}
			ul,ol{
				margin:0;
				padding:0;
				list-style:none;
			}  
	        a{
	        	text-decoration:none;
	        }  
	        img{
	        	border:none;
	        }  
	        canvas{
	        	background:#000;
	        	display: block;
	        } 
		</style>
	</head>
	<body>
		<canvas id="rain">你的浏览器不支持canvas标签，请使用另一个浏览器进行观看，谢谢！</canvas>
		<script type="text/javascript">
			//获取对象
			var rain = document.getElementById('rain');
			var yd = rain.getContext('2d');
			//获取浏览器窗口的宽度和高度，设置canvas为全屏显示
			var width = rain.width = window.innerWidth;
			var heigth = rain.height = window.innerHeight;
			//宽高响应式设计
			window.onresize = function(){
				width = rain.width = window.innerWidth;
				heigth = rain.height = window.innerHeight;
				console.log(width+' -- '+width/50);
			}
			var rains = [];//雨滴数组
			//创建雨滴对象
			function Rain(){};
			Rain.prototype = {
				init:function (){//初始化方法，设置雨滴的属性
					//雨滴水平方向上随机出现的范围
					this.x = random(0,width);
					//雨滴垂直方向上即浏览器的顶部出现
					this.y = 0;
					//Y方向的速度值
					this.vy = random(1,7);
					//雨滴下落的最大Y值
					this.l = random(0.8*heigth,0.9*heigth);
					//添加波纹半径
					this.r = 1;
					//波纹半径的扩散速度
					this.vr = 1;
					//判断雨滴消失的透明度
					this.a = 1;
					//透明度变化系数
					this.va = 0.96;
				},
				draw:function (){//绘制图形
					if(this.y > this.l){//雨滴已经下落到雨滴下落的最大Y值时,绘制波纹
						yd.beginPath();//先开始路径 ，每次绘制前，先提笔
						yd.arc(this.x,this.y,this.r,0,Math.PI*2,false);
						yd.strokeStyle = 'rgb(218,178,115)';
						yd.stroke();		
					}else{//绘制正在下落的雨滴
						yd.fillStyle = 'rgb(242,192,86)';
						yd.fillRect(this.x,this.y,2,10);
					}
					this.update();//每次绘制都更新
				},
				update:function (){//更新坐标位置
					if(this.y < this.l){
						this.y +=this.vy;						
					}else{//雨滴下落到指定位置
						if(this.a > 0.01){
							this.r += this.vr;
							if(this.r > 10){//半径大于50后，透明度会越来越大
								this.a *= this.va;
							}
						}else{
							this.init();//雨滴重新初始化
						}
					}
				},
			}
			//生成随机雨滴的方法
			function random(min,max){
				return Math.random()*(max-min)+min;//min-max之间的随机数
			}
			for(var i = 0;i <30;i++){
				setTimeout(function(){
					var drop = new Rain();
					drop.init();
					rains.push(drop);//添加到雨滴数组
				},i*500)
			}
			
			move();  
       		function move(){  
	            // cxt.clearRect();  
	            // 先绘制透明层，再绘制雨滴，雨滴就把先把先绘制的透明层覆盖，若干透明层叠加起来，就会越来也不透明  
	            yd.fillStyle = 'rgba(0,0,0,0.1)';  
	            yd.fillRect(0,0,width,heigth);  
	            for(var i=0;i<rains.length;i++){  
	                rains[i].draw();  
	            }  
	            requestAnimationFrame(move);  
        	}
		</script>
	</body>
</html>
