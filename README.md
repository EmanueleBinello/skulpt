# Welcome to Skulpt
Skulpt è una implementazione di Python 2.x in Javascript. 

### Building Skulpt

Prima di poter compilare ed utilizzare Skulpt è necessario avere i seguenti
prerequisiti installati:
1. node.js [vedi qui](https://nodejs.org/it/download)
2. python 2.x [vedi qui](https://www.python.it/download)

Quindi si può procedere come segue:
1. Clone the repository from GitHub, ideally using your own fork if you're planning on making any contributions
1. Clonare questo da GitHub con il comando:
```bash
git clone https://github.com/libremente/skulpt.git
```
2. Posizionarsi nella cartella appena creata dall'operazione di clone (tramite
   il comando `cd`.
3. Installare le dipendenze del progetto tramite `npm install`.
4. Eseguire `npm run build` per compilare il progetto.
5. Eseguire i test tramite `./skulpt.py test`.

A questo punto se i test sono passati correttamente nella cartella `dist`
dovrebbero esserci diversi file tra cui `skulpt.min.js` e `skulpt-stdlib.js`
che serviranno. 

Per lanciare un browser con un codice di prova è necessario eseguire il
comando:
`./skulpt.py brun`.

# Pagina di prova

La seguente pagina HTML di prova permette di usare Skulpt in locale. 

```html
<html> 
<head> 
<!-- Se si vogliono usare gli script remoti, abilitare questo -->
<!--<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.9.0/jquery.min.js" type="text/javascript"></script> -->
<!--<script src="http://www.skulpt.org/static/skulpt.min.js" type="text/javascript"></script> -->
<!--<script src="http://www.skulpt.org/static/skulpt-stdlib.js" type="text/javascript"></script> -->

<!-- Per usare gli script locali -->
<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.9.0/jquery.min.js" type="text/javascript"></script> 
<script src="dist/skulpt.min.js" type="text/javascript"></script> 
<script src="dist/skulpt-stdlib.js" type="text/javascript"></script> 

</head> 
<body> 

<script type="text/javascript"> 
// output functions are configurable.  This one just appends some text
// to a pre element.
function outf(text) { 
    var mypre = document.getElementById("output"); 
    mypre.innerHTML = mypre.innerHTML + text; 
} 
function builtinRead(x) {
    if (Sk.builtinFiles === undefined || Sk.builtinFiles["files"][x] === undefined)
            throw "File not found: '" + x + "'";
    return Sk.builtinFiles["files"][x];
}

// Here's everything you need to run a python program in skulpt
// grab the code from your textarea
// get a reference to your pre element for output
// configure the output function
// call Sk.importMainWithBody()
function runit() { 
   var prog = document.getElementById("yourcode").value; 
   var mypre = document.getElementById("output"); 
   mypre.innerHTML = ''; 
   Sk.pre = "output";
   Sk.configure({output:outf, read:builtinRead}); 
   (Sk.TurtleGraphics || (Sk.TurtleGraphics = {})).target = 'mycanvas';
   var myPromise = Sk.misceval.asyncToPromise(function() {
       return Sk.importMainWithBody("<stdin>", false, prog, true);
   });
   myPromise.then(function(mod) {
       console.log('success');
   },
       function(err) {
       console.log(err.toString());
   });
} 
</script> 

<h3>Try This</h3> 
<form> 
<textarea id="yourcode" cols="40" rows="10">import turtle

t = turtle.Turtle()
t.forward(100)

print "Hello World" 
</textarea><br /> 
<button type="button" onclick="runit()">Run</button> 
</form> 
<pre id="output" ></pre> 
<!-- If you want turtle graphics include a canvas -->
<div id="mycanvas"></div> 

</body> 
</html>
```

Questa pagina è presente nel repository e si chiama `pagina_di_prova.html` che
si può aprire nel browser una volta clonato il repository.
