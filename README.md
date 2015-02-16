# Signals.js

    A lightweight, super fast, pure javascript event system that works in node and the browser.

<h4>API</h4>&nbsp;<b> You can use it as a static mediator for signals.... </b>
    
      Set up a receiver channel....
    
        function callBack(d) {
            console.log(d);
        }
    
        Signals.receive("change", callBack);
        
<b>Or...</b>
            
        Signals.signaled("update", callBack);
        
    
<b>Then transmit it to the receiver(s)...</b>
    
        var payload = "I changed!";
    
        Signals.signal("change", payload);
        
            //  I changed!
        
        payload = "I have updates!";
        
        Signals.signal("update", payload);
        
           //  I have updates!
           
           
<b>Now kill a receiver channel...</b>

        Signals.dropReceivers("change");
        
        
<b>Or, drop all channels at once....</b>
    
        Signals.dropReceivers();


<b> Or use it to set up observable objects by using prototypal inheritance... </b>

<b> set up the base class... </b>
    
        function model() {
            var ID = null,
                slf = this;
            this.get = function() {
                return slf.ID;
            };
        };
    
        var mdl = model;
        
        
<b>  inherit from Signals....  </b>
    
        
        mdl.prototype = Signals;
        
        mdl.constructor = model.constructor;
        
        
<b> add a new method that emits the signal...  </b>    
        
        
        mdl.prototype.set = function(val) {
            var slf = this;
            if(val && val !== slf.ID) {
              slf.ID = val;
              slf.signal("change", val);
            }
            return slf;
        }
        
        
<b> create your observable object....  </b>    
        
        var Ident = new mdl();
    
    
<b> set up the receiver channel....  </b> 
        
        Ident.receive("change", function(d) {
            console.log("change signaled, new ID value: " + d);
        });


<b> assign a value to the ID property....  </b> 

        Ident.set("ABC");  //  "change signaled, new ID value: ABC"
        
<b> voila! the message confirms everything works. Now, check the ID...  </b>

        var lg = Ident.get();
        
        console.log(lg);  // "ABC"
        
 <b> and there ya go the new value is indeed there...</b>
 <b> try again with another value...</b>
           
        Ident.set(123);   //  "change signaled, new ID value: 123"
          
        lg = Ident.get();
        
        console.log(lg);  // 123
        
 <b> now kill the channels...  </b>
 
        Ident.dropReceivers();
        
 <h3> Thats all Yo!  </h3>
 
    
