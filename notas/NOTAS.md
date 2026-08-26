<link href="/home/cestien/memoria/template/nota_template/miestilo.css" rel="stylesheet"></link>

  <a href="https://github.com/cestien/diario/blob/main/diario/diario2025.md" target="_blank">Diario</a>
  
  <a href="https://github.com/cestien/Apuntes" target="_blank">Apuntes</a>

# Mechanistic Interpretability y Sparsed autoencoders 
El objetivo general del área de *mechanistic Interpretability* es hacer reverse-engineer acerca de cómo una red neuronal implementa computaciones específicas. Una de dichas áreas es la de *sparsed autoencoders* que se basa en descomponer las activaciones de la red en características (feautures) interpretables. 

* Interpretabilidad es el campo amplio — cualquier método para entender qué hace un modelo. Incluye cosas como analizar qué inputs activan una neurona, visualizaciones de atención, probing classifiers, etc. Es un campo con muchos enfoques, algunos bastante superficiales.
* Mecanicista es el adjetivo que especifica cómo se busca esa comprensión: no conformarse con correlaciones observables desde afuera ("esta neurona se activa más con palabras positivas") sino identificar los mecanismos causales concretos — qué operaciones matemáticas, en qué orden, producen qué resultado. La analogía que usa Chris Olah es la biología: no alcanza con describir que el corazón "bombea sangre", querés entender las válvulas, los impulsos eléctricos, la mecánica real.
La ingeniería inversa es la metodología: tomás un modelo ya entrenado (que nadie diseñó explícitamente, surgió del gradient descent) y tratás de reconstruir qué "algoritmo" aprendió. Es análogo a tomar un chip de silicio sin documentación y deducir su funcionamiento midiendo señales.
Lo que hace interesante y difícil el problema es que nadie diseñó esos algoritmos — emergieron solos del entrenamiento, así que no hay planos que consultar.
* La ingeniería inversa es la metodología: tomás un modelo ya entrenado (que nadie diseñó explícitamente, surgió del gradient descent) y tratás de reconstruir qué "algoritmo" aprendió. Es análogo a tomar un chip de silicio sin documentación y deducir su funcionamiento midiendo señales.
Lo que hace interesante y difícil el problema es que nadie diseñó esos algoritmos — emergieron solos del entrenamiento, así que no hay planos que consultar.

## Resúmenes propios
* <a href="/home/cestien/trabajo/zero-res/zrc/interpretability//notas/matrix_estoca.html" target="_blank">Notas de repaso de matemáticas.</a>
*  <a href="/home/cestien/trabajo/zero-res/zrc/early_language_adquisition/notas/NOTAS.html" target="_blank">Early language adquisition</a>
* [Toy models of superposition](./Toymodels.pdf)
## Papers principales y sitios

### Papers de Anthropic en Interpretability research
* [Thread: Circuits.](https://distill.pub/2020/circuits/) Viejo thread en distill.
* [Transformer Circuits Thread.](https://transformer-circuits.pub/). Nuevo thread en antrhopic.


### Sparsed autoencoders
* [Tutorial de Ng de autoencoders](http://ufldl.stanford.edu/tutorial/unsupervised/Autoencoders/)
* [Sparse Coding with an Overcomplete Basis Set: A Strategy Employed by V1 ?](../papers/sae/sparse_coding97.pdf)
* [Sparse autoencoders find highly interpretable features in language models](../papers/sae/sparse_autoencoders.pdf)
* [Taking features out of superposition with sparse autoencoders](https://www.alignmentforum.org/posts/z6QQJbtpkEAX3Aojj/interim-research-report-taking-features-out-of-superposition)
* [Efficient sparse coding algorithms](../papers/sae/sparse_coding06.pdf)
* [Paper de Nature de sparse autoencoders](../papers/sae/olshausen_field_nature_1996.pdf)
  
### Algunos sitios con simulaciones y herramientas de simulación
* [3Blue1Brown.](https://www.3blue1brown.com/) Sitio con videos de varios temas, muy didáctico
* [Notebooks de Anthropic](https://colab.research.google.com/github/anthropics/)
* [Código para Sparse autoencoders](https://github.com/HoagyC/sparse_coding)

* [SAELens. Herramienta analizar y entrenar SAEs](https://github.com/jbloomAus/SAELens)
* [ARENA.](https://learn.arena.education/)Cursos de ARENA
* [TransformerLens.](https://github.com/TransformerLensOrg/TransformerLens) A Library for Mechanistic Interpretability of Generative Language Models. [Documentación.](https://transformerlensorg.github.io/TransformerLens/)
* [Neel Nanda's Youtube channel](https://www.youtube.com/@neelnanda2469/playlists?view=1&sort=dd&shelf_id=5)
* [Mech Interp Paper Reading List](https://www.alignmentforum.org/posts/NfFST5Mio7BCAQHPA/an-extremely-opinionated-annotated-list-of-my-favourite-1)
* [Blog de Neel Nanda](https://www.neelnanda.io/top-posts)
* [A Comprehensive Mechanistic Interpretability Explainer & Glossary](https://dynalist.io/d/n2ZWtnoYHrU1s4vnFSAQ519J)
* [Transformer Lens Main Demo Notebook](https://colab.research.google.com/github/neelnanda-io/TransformerLens/blob/main/demos/Main_Demo.ipynb#scrollTo=V8UioPhv5uW9)
* [How To Become A Mechanistic Interpretability Researcher](https://www.alignmentforum.org/posts/jP9KDyMkchuv6tHwm/how-to-become-a-mechanistic-interpretability-researcher)
  

  
### Interpretabilidad en procesamiento de habla
* [Interpretability Techniques for Speech Models](https://interpretingdl.github.io/speech-interpretability-tutorial/). Tutorial de interspeech 2025
* [AudioSAE: Towards Understanding of Audio-Processing Models with
Sparse AutoEncoders](../papers/sae/sae_audio.pdf)
* [Sparse Autoencoders Make Audio Foundation Models More Explainable](../papers/sae/sae_explic.pdf)
* [Beyond Transcription: Mechanistic Interpretability in ASR](../papers/sae/beyond.pdf)
* [AudioSAE: Towards Understanding of Audio-Processing Models with Sparse AutoEncoders](../papers/reviews/sae/sae_audio.pdf)
* [Código para AudioSAE](https://github.com/audiosae/audiosae_demo)


## Notas del paper: Toy Models of Superposition 

### Definiciones previas

- **Feature**. Una feature es una dirección en el espacio de activaciones que representa un concepto que el modelo considera útil para disminuir su error de predicción. Características:
  - Es una variable latente. El modelo no recibe un concepto, recibe números, tokens o pixeles por ejemplo, pero no "automovil" o "verbo" o "furioso". Si la entrada es el token perro, podría contener features como "sustantivo" o "sentimiento de sarcasmo". 
  - En el paper se asume que la entrada física pasó por una capa intermedia que obtuvo las features y para cada entrada física particular (una imagen, un token, etc.) se obtiene  un vector de dimensión $n$ donde cada componente es un número entre cero y uno que indica la importancia de cada feature para esa entrada física. 
- **Superposición**. La feature es la unidad de información que el modelo quiere realmente representar. Pero una neurona en sí no representa necesariamente una feature. Podría haber muchas mas features que neuronas en cuyo caso una neurona podría activarse con la ocurrencia de muchas features. Esto es lo que llamamos *superposición*. Es un concepto geométrico, ocurre cuando se quieren representar features en direcciones que no son ortogonales, o se cuando se tienen mas features que neuronas, es decir cuando la dimensión del espacio de features $n$ es mayor que el espacio de activaciones $m$. 
- **Interferencia**. Es una consecuencia de la superposición. Ya que las direcciones no ortogonales van a tener proyecciones sobre  las direcciones ortogonales que interferirán con estas.
- **Sparsity**  
  - *Sparsity de una feature*. Es un concepto relacionado con la frecuencia de aparición de una feature en los datos de entrada. Una feature es sparse si se activa raramente. Es decir, la mayor parte de las veces en el tiempo vale cero. Por ejemplo, un verbo es una feature que ocurre muy frecuentemente en una oración, en cambio la palabra elefante ocurre en forma muy poco frecuente. 
  - *Sparsity de un vector*. Es un concepto relacionado con la representación de una feature en el espacio de las activaciones. Un vector es sparse si tiene muchos ceros en sus componentes.
  - En general el modelo asignará las $m$ direcciones que tiene a las features que sean mas frecuentes (menos sparsed) de modo que ocurra interferencia solamente con las features raras (poco frecuentes). 
  
  
### El modelo
- El modelo tendrá un espacio de activaciones de dimensión $m$ representado por una matriz de vectores columna ortogonal igual a la cantidad de neuronas. La asignación de direcciones a cada feature se realizará de acuerdo a su importancia y a su frecuencia de ocurrencia combinadas, de este modo:
  - Las $m$ mejores tendrán direcciones ortogonales por lo que no provocarán interferencia sobre otras direcciones. La matriz $W$ que define el modelo tendrá como columnas dichas direcciones.
  - Un conjunto de features intermedias que tendrán direcciones oblicuas las cuales por no ser tan frecuentes provocaran interferncia pero en forma ocasional
  - Un conjunto de features muy sparsed que se les asignará una dirección en el kernel de $W$ por lo que no tendrán representación en modelo. 
- Bases privilegiadas. Son las bases canónicas del espacio de las activaciones. Es decir cuando una feature se alinea con alguna de estas direcciones se activará solamente una neurona. El modelo elige las features más frecuentes para alinearlas con direcciones privilegiadas dado que si ocurren simultaneamente se estarían interfirendo entre sí todo el tiempo. 

#### Features y direcciones
Una representación con redes neuronales será lineal si las features se corresponden con direcciones en el espacio de activaciones. Cada feature $f_i$ tendrá una dirección correspondiente $W_i$. Solamente el mapeo de features a direcciones es lineal, las feaures que representa son funciones alineales de la entrada

La pregunta: Puede una red neuronal representar aunque sea con ruido mas feautures que neuronas tiene?. Con los modelos lineales parecería que no, solo se podrían almacenar los componentes principales. 

Queremos ver si una red neuronal puede proyectar un vector $x\in \mathbb{R}^n$ en un vector de menor dimensión  $h\in \mathbb{R}^m$ y recobrarlo. 

Tres características de las features:
- Son sparsed. Es decir su frecuencia de ocurrencia es baja. 
- Hay más features que neuronas. 
- Las features varían en importancia. 

 ### La superposición como cambio de fase
 Cuando se entrena un modelo pueden ocurrir tres cosas con una feature:
- Que la feature no sea aprendida
- Que sea aprendida y representada en superposición
- Que sea aprendida con una dimensión dedicada a ella

