# vue_slot
Vuejs—组件——slot内容分发



Slot分发内容

    ①概述：

     简单来说，假如父组件需要在子组件内放一些DOM，那么这些DOM是显示、不显示、在哪个地方显示、如何显示，就是slot分发负责的活。

   ②默认情况下

     父组件在子组件内套的内容，是不显示的。

     例如代码：
     
     显示内容是一个button按钮，不包含span标签里面的内容；
     
                 <div id="app">  
                    <children>  
                        <span>我是父组件</span>  
                        <!--上面这行不会显示-->  
                    </children>  
                </div>  
            <script>  
                var vm = new Vue({  
                    el: '#app',  
                    components: {  
                        children: {    //这个无返回值，不会继续派发  
                            template: "<button>我是子组件</button>"  
                        }  
                    }  
                });  
            </script>  
    
    
    ③单个slot
    
        简单来说，只使用这个标签的话，可以将父组件放在子组件的内容，放到想让他显示的地方。
    
             即父组件放在子组件里的内容，插到了子组件的<slot></slot>位置；

             注意，即使有多个标签，会一起被插入，相当于用父组件放在子组件里的标签，替换了<slot></slot>这个标签。
    
                    <div id="app">
                    <children>
                        <span>父组件</span>
                    </children>
                    </div>
                  <script>
                      var vm = new Vue({
                          el: '#app',
                          components: {
                              children: {    //这个无返回值，不会继续派发
                                  template: "<button><slot></slot>子组件</button>"
                              }
                          }
                      });
                  </script>
    
      生成dom    <button><span>父组件</span>子组件</button>
    
    
    
    
    
⑤分发内容的作用域：

        被分发的内容的作用域，根据其所在模板决定，例如，以上标签，其在父组件的模板中（虽然其被子组件的zizujian标签所包括，
        
        但由于他不在子组件的template属性中，因此不属于子组件），则受父组件所控制


         当点击文字vue.js的区域时（而不是按钮全部），会触发父组件的tobeknow方法。
         但是点击其他区域时则没有影响。
         
          <div id="app">
             <zizujian>
                 <!--父组件在要分发的标签里添加 slot=”name名” 属性-->
                 <span slot="one" @click="tobeknow">vue.js</span>
                 <span slot="twe">vue.jsp</span>
             </zizujian>

          </div>

          <script type="text/javascript">
            var vm=new Vue({
                el:"#app",
                methods:{
                 tobeknow:function(){
                   alert("我是父组件元素触发")
                 }
                },
              components:{
                zizujian:{
                //  子组件在对应分发的位置的slot标签里，添加name=”name名” 属性，
                    template:'<button><slot name="one"></slot>子组件<slot name="twe"></slot></button>'
                }
              }
            })
    
    ⑦假如想控制子组件根标签的属性

        【1】首先，由于模板标签是属于父组件的，因此，将子组件的指令绑定在模板标签里，是不可以的（因为他归属于父组件）；

        【2】假如需要通过父组件控制子组件是否显示（例如v-if或者v-show），那么这个指令显然是属于父组件的（例如放在父组件的data下面）。
             可以将标签写在子组件的模板上。

        如代码：
                      <div id="app">
                        <button @click="fun">点击</button>
                        <chin v-if="abc"></chin>
                    </div>


                    <script>
                        var vm=new Vue({
                          el:"#app",
                           data:{
                            abc:false
                          },
                           methods:{
                            fun:function(){
                             this.abc=!this.abc;
                            }
                           },
                          components:{
                          chin:{
                            template:'<div>我是子组件</div>'
                          }
                         }
                        })
                    </script>
    
      说明：

        通过父组件（点击按钮，切换v-if指令的值）控制子组件是否显示。
        
        
      【3】假如需要通过子组件，控制子组件是否显示（比如让他隐藏），那么这个指令显然是属于子组件的（会将值放在子组件的data属性下），
           那么就不能像上面这么写，而是必须放置在子组件的根标签中。  
           
           

                     <div id="app">  
                        <button @click="toshow">点击让子组件显示</button>  
                        <children>  
                            <span slot="first">【12345】</span>  
                            <!--上面这行不会显示-->  
                        </children>  
                    </div>  
                    <script>  
                        var vm = new Vue({  
                            el: '#app',  
                            methods: {  
                                toshow: function () {  
                                    this.$children[0].tohidden = true;  
                                }  
                            },  
                            components: {  
                                children: {    //这个无返回值，不会继续派发  
                                    template: "<div v-if='tohidden' @click='tohide'>这里是子组件</div>",  
                                    data: function () {  
                                        return {  
                                            tohidden: true  
                                        }  
                                    },  
                                    methods: {  
                                        tohide: function () {  
                                            this.tohidden = !this.tohidden;  
                                        }  
                                    }  
                                }  
                            }  
                        });  
                    </script>
           
           
           
        
        
    
    
