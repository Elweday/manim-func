# Manim Functional Approach 

- Every Scene is a function then converted to a class that has exactly one function `construct`
- Every Scene should contain only the animations and Scene-Specific-Mobjects
- Instead of Writing each `Tex Mobject` manually, using the built-in functionality of sympy is easier <b>But it has less control</b>
- Every recurring pattern in the code should be abstracted functionally
- This approach can be code-heavy
- It can also used alongside with `ffmpeg` and `SoX` to merge scenes and deal with audio libs for just-code experience
- `Sympy` has a lot potential to work along side with manim:
    <details>
  <summary> What can it do ? </summary>
    - Algebra: Symbolic manipulation, equation solving, polynomial manipulation, simplification, factorization, expansion, substitution, etc.

    - Calculus: Differentiation, integration, limits, series expansion, Taylor series, differential equations, numerical integration, etc.

    - Number Theory: Prime numbers, factorization, modular arithmetic, continued fractions, divisibility testing, prime factorization, etc.

    - Discrete Mathematics: Permutations, combinations, binomial coefficients, partitions, graph theory, matrix representation, random variables, probability distributions, etc.

    - Geometry: Points, lines, circles, polygons, angles, intersections, transformations, 2D plotting, 3D plotting, geometric algorithms, etc.

    - Linear Algebra: Matrices, matrix operations, determinants, eigenvalues, eigenvectors, matrix factorization, matrix inversion, solving linear systems, least squares, etc.

    - Combinatorics: Combinatorial enumeration, permutations, combinations, partitions, generating functions, Stirling numbers, Catalan numbers, etc.

    - Statistics: Probability distributions, random variables, statistical functions, statistical testing, hypothesis testing, confidence intervals, etc.

    - Physics: Mechanics, classical physics, quantum physics, statistical mechanics, optics, electromagnetism, quantum computing, etc.

    - Differential Geometry: Manifolds, curvature, tensors, coordinate systems, differential forms, Lie derivatives, etc.

    - Logic: Propositional logic, predicate logic, logical operators, truth tables, logic gates, logical equivalences, Boolean algebra, etc.

    - Complex Analysis: Complex numbers, complex functions, complex integration, residues, contour integration, etc.
</details>

    

<b>Learn more about [Sympy](https://docs.sympy.org/latest/index.html) </b>


```python
from manim import *
import sympy as sp
```

## Variable definitions and utilites


```python
x,n = sp.symbols("x n")
def sequencer(expr, var):
  """Turns a Sympy expression into a funtion for mapping"""
  def create_mobject(nTerms):
    """The tex mobject that would be transformed"""
    return Tex("$ "+ sp.latex(sp.sequence(expr, (var,0,nTerms))) +" $")
  return create_mobject

def Transfrom(mobs, Transform_Func = TransformMatchingShapes):
  """Generator that takes an iterable of mobjects and returns a generator of transformations"""
  mob1 = next(mobs)
  for mob2 in mobs:
    yield Transform_Func(mob1, mob2)
    mob1 = mob2
```

## Scene1 

This is a fully-functioning scene


```python
def S_tex(self):
  mobs = map(sequencer(x**n + 5*x), range(0,4))
  for t in Transfrom(mobs, Transform_Func = TransformMatchingShapes):
    self.play(t)
```



### Scenes can be sorted and manipulated, combined with metadata



```python
Scenes = {
  S_tex:{
    "render" : True,
    "Scene_Type":Scene,
    "Some Scene Meta Data" : 100_000_000
  }
}
```
#### Converting all the scene functions to classes (not exactly), but it does work as intended


```python
for constructor, meta in Scenes.items():
  SceneName = constructor.__name__
  globals()[SceneName]  = type(SceneName,(meta["Scene_Type"], ),{"construct": constructor})
  if meta["render"]:
    %manim -ql -v WARNING --disable_caching $SceneName

```

                                                                                                                           

#### Result

https://github.com/Elweday/manim-func/assets/100801944/b3d8d221-076f-4320-ae33-209959cf3110

#### It is not much, But it's about the possiblities 

