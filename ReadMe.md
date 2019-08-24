# GScience Virtual Machine (GVM)
## 1. What is GVM
An virtual machine like JVM, dot Net CRL but simpler. The project is created just for fun, and maybe with a lot of bugs. Please just turn around and leave if you think the whole project is useless.

I decided to add multiplatform supportion, which means that with one compilation the program can run in different platforms.
### Support platform
- Windows
More platform supportion will be added.
<br/>

## 2. Resources
- <a href="#Instruction-set">Instruction set</a>
- VM model
- <a href="#Introduction-of-GCode">Introduction of GCode</a>
<br/>

### Instruction set
|Instruction Code|Arguements|Description|
| ------------ | ------------ | ------------ |
|<center>Push.I8</center>	|<center>Int8</center>	|Push an int8 object to the top of the evaluation stack.|
|<center>Push.I16</center>	|<center>Int16</center>	|Push an int16 object to the top of the evaluation stack.|
|<center>Push.I32</center>	|<center>Int32</center>	|Push an int32 object to the top of the evaluation stack.|
|<center>Push.Str</center>	|<center>String</center>|Push a string object to the top of the evaluation stack.|
|<center>Push.Bool</center>	|<center>Bool</center>	|Push a bool object to the top of the evaluation stack.|
|<center>Push.Ref</center>	|<center>Ref</center>	|Push a reference object to the top of the evaluation stack.|
|<center>Push.Arr</center>	|<center>Type Int32</center>	|Push a array object of give type to the top of the evaluation stack.|
|<center>Pop</center>		|<center>None</center>	|Remove an object from the top of the evaluation stack.|
|<center>Ld.This</center>	|<center>None</center>	|Push the reference of current object to the top of the evaluation stack.|
|<center>Ld.Loc</center>	|<center>Int8</center>	|Push the local variable with given id to the top of the evaluation stack.|
|<center>Ld.Field</center>	|<center>Field</center>	|Push the value stored in a field of an object to the top of the evaluation stack.|
|<center>Ld.Ele</center>	|<center>Int32</center>	|Push the element at given index of array to the top of the evaluation stack with specified type.|
|<center>St.Loc</center>	|<center>Int8</center>	|Pop an object from the top of the evaluation stack and store it in a local variable with given id.|
|<center>St.Field</center>	|<center>Field</center>	|Replace the value stored in a field of an object with a new value.|
|<center>Sd.Ele</center>	|<center>Int32</center>	|Replace the value of the element at given index of array with a new value.|
|<center>Call</center>		|<center>Function</center>	|Call a function|
|<center>Ret</center>		|<center></center>			|Return from the function|
......

### Introduction of GCode
#### What is GCode
GCode is a kind of intermediate language, which can be directly compiled to byte code. We provide some necessary tools for you to ccompile and decompile GCode. 

#### Grammar
There is an example: 
    
    //Define a class
    .class example
        //Define a field
        .field -public int exampleField
        //Define a field with type defined in lib [CoreLib]
        .field -private [CoreLib]TypeInExternalLib anotherField
        
        //Define a constructor function
        .function -public $constructor
            //Push this to the evaluation stack
            Ld.This
            //Push 10 to the evaluation stack
            Push.I16 10
            //Set the value of exampleField to 10
            St.Field Field:example.exampleField
            
            Ld.This
            Push.I16 10
            Push.Bool false
            //Call example.MyFirstFunction(10, false)
            call Function:example.MyFirstFunction
            
            //Call static example.MyFirstFunction()
            call Function:example.MyFirstFunction
            //return
            ret
        //End of constructor function
        .end
        
        .function -public MyFirstFunction
            //First arg is an integer
            .loc int
            //Second arg is a bool
            .loc bool
            ret
        .end
        
        .function -public -static MyFirstFunction
            ret
        .end
    //End of class
    .end


