# Tensors

{{#paragraph}}
  This chapter will define the notion of a <em>tensor</em> and develop
  the toolset needed for the study of tensor networks. We will start with 
  a motivational section to explain the goal that is formulating a 
  natural extension of Linear Algebra terms as vectors or linear maps.
  Then we will study operations on tensors that serve for their 
  construction and modification. We have dedicated sections on 
  <em>tensor products</em>, <em>grouping</em> and <em>contraction</em>. 
  Next we will outline how tensors and multilinear maps are associated.
  We conclude this chapter with an overview of the basic tensor terms and
  their relations - this ought to serve as glossary for later chapters.
{{/paragraph}}

{{#section "secBackground"}}

  {{#paragraph}}
    A <em>field</em> {{#latex}}\F{{/latex}} can also be considered
    as a vector space (over {{#latex}}\F{{/latex}}). 
    Indeed, the inner addition fulfills all axioms of
    vector space addition - and the inner multiplication fulfills 
    all axioms of vector space scalar multiplication.
    An element {{#latex}}a\in\F{{/latex}} would then play the role
    of a vector. Furthermore it could also be seen as a 
    linear map of {{#latex}}\cL(\F){{/latex}}
    by {{#latex}}x\mapsto a\cdot x{{/latex}}. Choosing the basis
    {{#latex}}\{1\}{{/latex}}, we can express {{#latex}}a{{/latex}}
    with component {{#latex}}a^1\def a{{/latex}} and
    {{#latex}}[a^1]{{/latex}} would denote the vector components or 
    matrix for {{#latex}}a{{/latex}} seen as vector of likewise
    linear map.
  {{/paragraph}}
 
  {{#paragraph}}
    Let {{#latex}}V{{/latex}} be (another) finite dimensional 
    <em>vector space</em> over {{#latex}}\F{{/latex}}.
    Let {{#latex}}v\in V{{/latex}} and {{#latex}}v^i{{/latex}}
    denote its components in basis {{#latex}}\{e_1,\ldots,e_n\}{{/latex}}.
    Obviously, {{#latex}}v{{/latex}} is a vector, but can also be
    considered as linear map of {{#latex}}\cL(V,\F){{/latex}} by 
    {{#latex}}
      x=\sum\limits_{i=1}^nx^ie_i\mapsto\sum\limits_{i=1}^nx^iv^i
    {{/latex}}.
    The transposed component vector {{#latex}}[v_1\ldots v_n]{{/latex}}
    encodes the matrix representation of this linear map in bases
    {{#latex}}\{e_1,\ldots,e_n\}{{/latex}} and 
    {{#latex}}\{1\}{{/latex}}.
  {{/paragraph}}

  {{#paragraph}}
    Consider a linear map {{#latex}}T\in\cL(V,W){{/latex}}
    between finite dimensional vector spaces 
    {{#latex}}V{{/latex}} and {{#latex}}W{{/latex}} 
    over {{#latex}}\F{{/latex}}.
    We know that the set of all such linear maps 
    {{#latex}}\cL(V,W){{/latex}} forms a vector space 
    itself. Again, on one hand {{#latex}}T{{/latex}} can 
    be seen as vector and on the other hand of course
    as linear map. Fixing bases 
    {{#latex}}\{e_1,\ldots,e_n\}\subseteq V{{/latex}} and 
    {{#latex}}\{f_1,\ldots,f_m\}\subseteq W{{/latex}} we 
    obtain the matrix representation 
    {{#latex}}[T^{ij}]_{i=1,j=1}^{n\;\;\;\;m}{{/latex}} 
    of the linear map {{#latex}}T{{/latex}}. Likewise this 
    would also serve as component description of 
    {{#latex}}T{{/latex}} regarded as vector.
  {{/paragraph}}

  {{#paragraph}}
    Summarizing, Linear Algebra knows different kinds of 
    objects that behave linearly. We have:
  {{/paragraph}}

  <ul>
    <li>
      <em>Scalars</em> {{#latex}}a,b,\ldots\in\F{{/latex}} can be 
      seen as vectors as well as linear maps. Their components descriptions 
      are again scalars (no index needed), which can be interpreted 
      as 0-dimensional arrays.
    </li>
    <li>
      <em>Vectors</em> {{#latex}}v,w,\ldots\in V{{/latex}} can be 
      seen as vectors as well as linear maps. Their componts descriptions
      need one index and can be interpreted as 1-dimensional arrays.
    </li>
    <li>
      <em>Linear maps</em> {{#latex}}S,T,\ldots\in\cL(V,W){{/latex}} can be 
      seen as vectors as well as linear maps. Their matrix representations
      need two indices and can be interpreted as 2-dimensional arrays.
    </li>
  </ul>

  {{#paragraph}}
    The notion of a <em>tensor</em> will uniform these objects and generalize 
    them into a framework that incorporates representations of arbitrary many indices.
    Linearity will be preserved, thus making tensors, being interpreted as 
    vectors or as multilinear maps an expressive tool for e.g. Physicists.
    Furthermore, their components representations being interpreted as 
    multidimensional arrays prove to be useful in Computer Science applications 
    as Machine Learning.
  {{/paragraph}}

{{/section}}

{{#section "secProduct"}}

  {{#paragraph}}
    We will construct tensors out of tensors with less indices. So we can start
    with ordinary vectors and combine them to tensors. Those again being interpreted
    as vectors can then be used to construct tensors with even more indices.
    This construction rule will be given by the <em>tensor product</em> that combines
    vectors {{#latex}}v\in V,\,w\in W{{/latex}} in vector spaces 
    {{#latex}}V,W{{/latex}} over the same field {{#latex}}\F{{/latex}} to 
    a vector {{#latex}}v\otimes w{{/latex}} in a vector space 
    {{#latex}}V\otimes W{{/latex}} over {{#latex}}\F{{/latex}}.
    This product will be commutative, associative, distributive over vector addition
    and interchangable with scalar multiplication.
  {{/paragraph}}

  {{#paragraph}}
    To motivate the upcoming definition we will have a closer look to distributivity over
    vector addition and interchangability with scalar multiplication.
    Together with the map 
    {{#latex}}\varphi:V\times W\rightarrow V\otimes W,\,\varphi(v,w)\def v\otimes w{{/latex}}
    and
    {{#latex}}a\in\F,\,v,v_1,v_2\in V,\,w,w_1,w_2\in W{{/latex}}
    this implies the following equations.
  {{/paragraph}}

  {{#latexeqn_}}
    & \varphi(v_1+v_2,w)=(v_1+v_2)\otimes w = v_1\otimes w + v_2\otimes w = \varphi(v_1,w)+\varphi(v_2,w) \\
    & \varphi(a\cdot v,w)=(a\cdot v)\otimes w = a\cdot(v\otimes w) = a\cdot \varphi(v,w) \\    
    & \varphi(v,w_1+w_2)=v\otimes(w_1+w_2) = v\otimes w_1 + v\otimes w_2 = \varphi(v,w_1)+\varphi(v,w_2) \\
    & \varphi(v,a\cdot w)=v\otimes (a\cdot w) = a\cdot(v\otimes w) = a\cdot \varphi(v,w)
  {{/latexeqn_}}

  {{#paragraph}}
    We see, that both desired properties of the tensor product require the function
    {{#latex}}\varphi{{/latex}} to be linear in both arguments, i.e. being bilinear. This explains 
    the following definition.
  {{/paragraph}}

  {{#definition "defProduct"}}

    {{#paragraph}}
      Let {{#latex}}V,W{{/latex}} be vector spaces over a field 
      {{#latex}}\F{{/latex}} with bases
      {{#latex}}
        \{e_1,\ldots,e_n\}\subseteq V,\,\{f_1,\ldots,f_m\}\subseteq W
      {{/latex}}.
      For each combination of two basis elements 
      {{#latex}}e_i\in V,\,f_j\in W{{/latex}} we define a symbol
      {{#latex}}e_i\otimes f_j{{/latex}}. The set of all symbols
      {{#latex}}\{e_i\otimes f_j:1\le i\le n,\,1\le j\le m\}{{/latex}} 
      forms a basis of the {{#highlight}}tensor product space{{/highlight}}
      {{#latex}}V\otimes W{{/latex}} and we define
    {{/paragraph}}

    {{#latexeqn_}}
      V\otimes W\def \span\{e_i\otimes f_j:1\le i\le n,\,1\le j\le m\}\,.
    {{/latexeqn_}}

    {{#paragraph}}
      The {{#highlight}}tensor product{{/highlight}} of two vectors 
      is given by a bilinear embedding map 
      {{#latex}}\varphi:V\times W\rightarrow V\otimes W{{/latex}} with
    {{/paragraph}}

    {{#latexeqn_}}
      \varphi(v_i,w_j)=v_i\otimes w_j,\,1\le i\le n,\,1\le j\le m\,.
    {{/latexeqn_}}

    {{#paragraph}}
      For {{#latex}}v\in V{{/latex}} and {{#latex}}w\in W{{/latex}} 
      we define
    {{/paragraph}}

    {{#latexeqn_}}
      v\otimes w\def\varphi(v,w)\,.
    {{/latexeqn_}}

    {{#paragraph}}
      If {{#latex}}V{{/latex}} and {{#latex}}W{{/latex}} are inner product 
      spaces, {{#latex}}V\otimes W{{/latex}} is also an inner product space 
      with 
    {{/paragraph}}
    
    {{#latexeqn_}}
      \ip{e_i\otimes f_j}{e_p\otimes f_q}_{V\otimes W}\def
        \ip{e_i}{e_p}_V\cdot\ip{f_j}{f_q}_W\,,\quad
        i,p\in\{1,\ldots,n\},\;j,q\in\{1,\ldots,m\}\,.
    {{/latexeqn_}}

  {{/definition}}

  {{#paragraph}}
    Directly from construction of {{#latex}}V\otimes W{{/latex}} 
    (and its basis) we obtain the dimension of the tensor product space.
  {{/paragraph}}

  {{#theorem "obsDimension"}}

    {{#latexeqn_}}
      \dim(V\otimes W)=\dim V\cdot\dim W
    {{/latexeqn_}}

  {{/theorem}}

  {{#paragraph}}
    In general a tensor {{#latex}}T\in V\otimes W{{/latex}} can be 
    expressed in chosen combined basis as
  {{/paragraph}}

  {{#latexeqn "eqnTensorBasis"}}
    T=\sum\limits_{i=1}^n\sum\limits_{j=1}^mT^{ij}\,e_i\otimes f_j
  {{/latexeqn}}

  {{#paragraph}}
    with {{#latex}}T^{ij}\in\F{{/latex}}. The components can be interpreted as 
    matrix {{#latex}}[T^{ij}]_{i=1,j=1}^{n\;\;\;\;m}{{/latex}}.
    Products of vectors from {{#latex}}V{{/latex}} and {{#latex}}W{{/latex}} are 
    embedded into {{#latex}}V\otimes W{{/latex}} by means of 
    {{#latex}}\varphi{{/latex}}. The basis representation of
    {{#latex}}v\otimes w{{/latex}} for 
    {{#latex}}v=\sum\limits_{i=1}^nv^ie_i\in V{{/latex}} and 
    {{#latex}}w=\sum\limits_{j=1}^mw^je_j\in W{{/latex}} 
    is (using bilinearity of {{#latex}}\varphi{{/latex}})
  {{/paragraph}}

  {{#latexeqn "eqnProductBasis"}}
    v\otimes w 
    &= \Bigg(\sum\limits_{i=1}^nv^ie_i\Bigg) \otimes \Bigg(\sum\limits_{j=1}^mw^je_j\Bigg) \\
    &= \sum\limits_{i=1}^n\sum\limits_{j=1}^mv^iw^j\,e_i\otimes f_j\,.
  {{/latexeqn}}

  {{#paragraph}}
    Seen from opposite direction, the tensor 
    {{#latex}}
      v\otimes w=\sum\limits_{i=1}^n\sum\limits_{j=1}^m(v\otimes w)^{ij}\,e_i\otimes f_j
    {{/latex}}
    has a decomposition into {{#latex}}v{{/latex}} and {{#latex}}w{{/latex}}
    with 
  {{/paragraph}}

  {{#latexeqn "eqnProductComponents"}}
    (v\otimes w)^{ij}=v^iw^j\,,
  {{/latexeqn}}

  {{#paragraph}}
    or written as matrix equation
  {{/paragraph}}

  {{#latexeqn "eqnProductMatrix"}}
    \begin{bmatrix} 
      (v\otimes w)^{11} & \cdots & (v\otimes w)^{1m} \\
      \vdots & \ddots & \vdots \\
      (v\otimes w)^{n1} & \cdots & (v\otimes w)^{nm}
    \end{bmatrix}
    =
    \begin{bmatrix} 
      v^1 \\
      \vdots \\
      v^n
    \end{bmatrix}
    \begin{bmatrix} 
      w^1 & \cdots & w^m
    \end{bmatrix}
  {{/latexeqn}}

  {{#paragraph}}
    Not all tensors from {{#latex}}V\otimes W{{/latex}} can be decomposed this way.
  {{/paragraph}}

  {{#example "exSeparation"}}

    {{#paragraph}}
      We choose {{#latex}}V\def W\def\R^2{{/latex}}, both with standard basis
      {{#latex}}\{e_1\def [1\;\;0]^T,\,e_2\def [0\;\;1]^T\}{{/latex}} 
      and consider the tensors
    {{/paragraph}}

    {{#latexeqn_}}
      &S=e_1\otimes e_1+e_2\otimes e_1\,,\\
      &T=e_1\otimes e_1+e_2\otimes e_2\,.
    {{/latexeqn_}}

    {{#paragraph}}
      Tensor {{#latex}}S{{/latex}} can be written as product 
      {{#latex}}S=(e_1+e_2)\otimes e_1{{/latex}}. This can be 
      obtained by applying the distributive law 
      (bilinearity of {{#latex}}\varphi{{/latex}}) and simply 
      multiplying out. Another way would be to validate the 
      respective matrix equation
    {{/paragraph}}

    {{#latexeqn_}}
      \begin{bmatrix} 
        1 & 0 \\
        1 & 0
      \end{bmatrix}
      =
      \begin{bmatrix} 
        1 \\
        1
      \end{bmatrix}
      \begin{bmatrix} 
        1 & 0
      \end{bmatrix}
      \,.
    {{/latexeqn_}}

    {{#paragraph}}
      We see also, that the product decomposition is not unique. Due to interchangability 
      with scalar multiplication (bilinearity of {{#latex}}\varphi{{/latex}}), we 
      can shift a scalar factor from one side to the other, e.g. 
      {{#latex}}S=2(e_1+e_2)\otimes 2^{-1}e_1{{/latex}}.
    {{/paragraph}}

    {{#paragraph}}
      Tensor {{#latex}}T{{/latex}} can not be written as a tensor product 
      of two vectors. The ansatz
    {{/paragraph}}

    {{#latexeqn_}}
      \begin{bmatrix} 
        1 & 0 \\
        0 & 1
      \end{bmatrix}
      =
      \begin{bmatrix} 
        v^1 \\
        v^2
      \end{bmatrix}
      \begin{bmatrix} 
        w^1 & w^2
      \end{bmatrix}
    {{/latexeqn_}}

    {{#paragraph}}
      yields {{#latex}}v^1w^1=v^2w^2=1{{/latex}}, which demands that all components 
      {{#latex}}v^i{{/latex}} and {{#latex}}w^j{{/latex}} are different from zero. 
      But this contradicts {{#latex}}v^1w^2=0{{/latex}}.
    {{/paragraph}}

  {{/example}}

  {{#paragraph}}
    This means, the following definition is sensible.
  {{/paragraph}}

  {{#definition "defEntanglement"}}
    
    {{#paragraph}}
      A tensor {{#latex}}T\in V\otimes W{{/latex}} is called 
      {{#highlight}}separable{{/highlight}} or 
      {{#highlight}}unentangled{{/highlight}}, if vectors
      {{#latex}}v\in V,\,w\in W{{/latex}} exist, such that 
      {{#latex}}T=v\otimes w{{/latex}}.
      Otherwise {{#latex}}T{{/latex}} is called 
      {{#highlight}}entangled{{/highlight}}.
    {{/paragraph}}

  {{/definition}}

  {{#paragraph}}
    The term <em>entangled</em> is motivated from Quantum Mechanics. 
    We will leave it here with briefly mentioning the background. 
    A system of several particle states, e.g. two electron spins, is modelled as tensor.
    The particle states behave correlatedly, exactly when this tensor 
    cannot be decomposed into a tensor product of individual particle states.
  {{/paragraph}}

  {{#paragraph}}
    In the beginning of this section we have stated, that the tensor product 
    shall be commutative and associative. Obviously it is not commutative.
    E.g. in the setting of {{ref "tensors" "exSeparation"}} we have to admit that
  {{/paragraph}}

  {{#latexeqn_}}
    e_1\otimes e_2\neq e_2\otimes e_1
  {{/latexeqn_}}

  {{#paragraph}}
    or as matrix equation
  {{/paragraph}}  

  {{#latexeqn_}}
    \begin{bmatrix} 
      1 \\
      0
    \end{bmatrix} 
    \begin{bmatrix} 
      0 & 1 \\
    \end{bmatrix} 
    =
    \begin{bmatrix} 
      0 & 1 \\
      0 & 0
    \end{bmatrix} 
    \neq
    \begin{bmatrix} 
      0 & 0 \\
      1 & 0
    \end{bmatrix} 
    =
    \begin{bmatrix} 
      0 \\
      1
    \end{bmatrix} 
    \begin{bmatrix} 
      1 & 0 \\
    \end{bmatrix} 
    \,.
  {{/latexeqn_}}

  {{#paragraph}}
    But there is a natural isomorphism between {{#latex}}V\otimes W{{/latex}}
    and {{#latex}}W\otimes V{{/latex}} given by this mapping of all basis elements:
  {{/paragraph}}

  {{#latexeqn "eqnCommutative"}}
    e_i\otimes f_j\longleftrightarrow f_j\otimes e_i\,.
  {{/latexeqn}}

  {{#paragraph}}
    This isomorphism swaps the indices of the tensor, i.e.
  {{/paragraph}}

  {{#latexeqn "eqnCommutative2"}}
    \sum\limits_{i=1}^n\sum\limits_{j=1}^m T^{ij}\,e_i\otimes f_j
    \longleftrightarrow 
    \sum\limits_{j=1}^m\sum\limits_{i=1}^n \tilde{T}^{ji}\,f_j\otimes e_i
  {{/latexeqn}}

  {{#paragraph}}
    with {{#latex}}T^{ij}=\tilde{T}^{ji}{{/latex}}. Hence, the tensors are 
    essentially the same, apart from transposed coefficients matrices.
    Keeping in mind, that we actually have 
    {{#latex}}V\otimes W\simeq W\otimes V{{/latex}}, we will identify both 
    tensor product spaces and make the tensor product commutative.
  {{/paragraph}}

  {{#paragraph}}
    In a similar way we will identify
    {{#latex}}(V\otimes W)\otimes X{{/latex}} with
    {{#latex}}V\otimes (W\otimes X){{/latex}} by 
    utilizing the isomorphism
  {{/paragraph}}

  {{#latexeqn "eqnAssociative"}}
    (e_i\otimes f_j)\otimes g_k\longleftrightarrow e_i\otimes (f_j\otimes g_k)
  {{/latexeqn}}

  {{#paragraph}}
    with {{#latex}}\{g_1,\ldots,g_p\}{{/latex}} being a basis of 
    {{#latex}}X{{/latex}}. This leads to an associative tensor product.
    As common for associative products (which means that multiplication order
    does not matter), we can and will omit paranthesis completely and write e.g. 
    {{#latex}}V\otimes W\otimes X{{/latex}}.
  {{/paragraph}}

  {{#paragraph}}
    We have succeeded in finding a way to combine vectors
    into tensors, such that the underlying product is commutative, 
    associative, distributive over vector addition and interchangable with 
    scalar multiplication. Since the tensor product space is a vector 
    space itself, we can continue to further construct tensors with even more
    indices. An element of {{#latex}}\R^2\otimes\R^2\otimes\R^2{{/latex}} reads 
    in general (using standard bases as given in {{ref "tensors" "exSeparation"}})
  {{/paragraph}}

  {{#latexeqn_}}
    \sum\limits_{i=1}^2
    \sum\limits_{j=1}^2
    \sum\limits_{k=1}^2
      T^{ijk}\,e_i\otimes e_j\otimes e_k\,.
  {{/latexeqn_}}

  {{#paragraph}}
    The coefficients form a 3-dimensional array, e.g. the tensor
  {{/paragraph}}

  {{#latexeqn_}}
    3\,e_1\otimes e_1\otimes e_1 +
    6\,e_1\otimes e_1\otimes e_2 +
    4\,e_2\otimes e_2\otimes e_1 +
    8\,e_2\otimes e_2\otimes e_2
  {{/latexeqn_}}

  {{#paragraph}}
    is represented by the following structure.
  {{/paragraph}}

  {{imageHeight "tensors-order-3.svg" "150em"}}

  {{#paragraph}}
    Dual vector spaces play a prominent role in tensor products. 
    Therefore they will be explicitly noted.
    The general tensor product space is of form
  {{/paragraph}}

  {{#latexeqn "eqnGenProdSpace"}}
    W_1\otimes\ldots\otimes W_r\otimes V^\ast_1\otimes\ldots\otimes V^\ast_s
  {{/latexeqn}}

  {{#paragraph}}
    with all {{#latex}}W_p{{/latex}} being vector spaces over a field 
    {{#latex}}\F{{/latex}}, and all {{#latex}}V^\ast_q{{/latex}} being dual 
    spaces over {{#latex}}\F{{/latex}} (to vector 
    spaces {{#latex}}V_q{{/latex}} over {{#latex}}\F{{/latex}}).
  {{/paragraph}}

  {{#definition "defTensorOrder"}}

    {{#paragraph}}
      The coefficients of a tensor
      {{#latex}}
        T\in W_1\otimes\ldots\otimes W_r\otimes V^\ast_1\otimes\ldots\otimes V^\ast_s
      {{/latex}}
      are identified by {{#latex}}r+s{{/latex}} indices. 
    {{/paragraph}} 
      
    {{#paragraph}}
      We call indices belonging to 
      vector spaces {{#latex}}W_1,\ldots,W_r{{/latex}}
      {{#highlight}}contravariant{{/highlight}} indices.
      Indices belonging to dual spaces {{#latex}}V^\ast_1,\ldots,V^\ast_s{{/latex}} 
      are called {{#highlight}}covariant{{/highlight}} indices.
    {{/paragraph}}

    {{#paragraph}}
      Contravariant indices will be written in superscript at coefficients and 
      in subscript at basis elements. Vice versa, covariant indices will be 
      written in subscript at coefficients and in superscript at basis elements.  
      Having bases {{#latex}}\{f^{(p)}_1,\ldots,f^{(p)}_{m_p}\}{{/latex}} of 
      {{#latex}}W_p{{/latex}} and bases 
      {{#latex}}\{e^{(q)1},\ldots,e^{(q)n_q}\}{{/latex}} of 
      {{#latex}}V^\ast_q{{/latex}}, we would write
    {{/paragraph}}

    {{#latexeqn_}}
      T=
      \sum\limits_{j_1=1}^{m_1}\cdots
      \sum\limits_{j_r=1}^{m_r}
      \sum\limits_{i_1=1}^{n_1}\cdots
      \sum\limits_{i_s=1}^{n_s}
      T^{j_1\ldots j_r}_{i_1\ldots i_s}\,
      f^{(1)}_{j_1}\otimes\ldots\otimes f^{(r)}_{j_r}\otimes
      e^{(1){i_1}}\otimes\ldots\otimes e^{(s){i_s}}
      \,.
    {{/latexeqn_}}

    {{#paragraph}}
      A tensor with {{#latex}}r{{/latex}} contravariant and 
      {{#latex}}s{{/latex}} covariant indices is called
      an {{#highlight}}order-{{/highlight}}{{#latex}}\bm{(r,s)}{{/latex}}
      {{#highlight}}tensor{{/highlight}}.
    {{/paragraph}}

  {{/definition}}

  {{#paragraph}}
    Let us come back to the standard objects of Linear Algebra. 
    They smoothly fit into the general tensor language.
  {{/paragraph}}

  <ul>
    <li>
      <em>Vectors</em> are order-{{#latex}}(1,0){{/latex}} tensors.
    </li>
    <li>
      <em>Dual vectors</em> are order-{{#latex}}(0,1){{/latex}} tensors.
    </li>
    <li>
      <em>Linear maps</em> are order-{{#latex}}(1,1){{/latex}} tensors.
      This will be explained in section {{ref "tensors" "secMultilinear"}}.
    </li>
    <li>
      <em>Scalars</em> are order-{{#latex}}(0,0){{/latex}} tensors.
      This is justified by the fact that a scalar does not require 
      an index at all (although it might be indexed by a constant).
    </li>
  </ul>

  {{#paragraph}}
    We conclude this section by turning our attention back to 
    {{ref "tensors" "defProduct"}} and showing that the definition of
    the inner product for basis elements extends naturally to all 
    tensor products.
  {{/paragraph}}

  {{#theorem "obsInnerProduct"}}

    {{#paragraph}}
      In the setting of {{ref "tensors" "defProduct"}} we have for
      {{#latex}}v_1,v_2\in V{{/latex}} and {{#latex}}w_1,w_2\in W{{/latex}} 
      the identity
    {{/paragraph}}

    {{#latexeqn_}}
      \ip{v_1\otimes w_1}{v_2\otimes w_2}_{V\otimes W}
      =
      \ip{v_1}{v_2}_V\cdot\ip{w_1}{w_2}_W\,.
    {{/latexeqn_}}

  {{/theorem}}

  {{#proof}}

    {{#paragraph}}
      With 
      {{#latex}}
        v_1=\sum\limits_{i=1}^nv_1^ie_i,\,
        v_2=\sum\limits_{p=1}^nv_2^pe_p,\,
        w_1=\sum\limits_{j=1}^mw_1^jf_j,\,
        w_2=\sum\limits_{q=1}^mw_2^qf_q
      {{/latex}}
      we obtain
    {{/paragraph}}

    {{#latexeqn_}}
      \ip{v_1\otimes w_1}{v_2\otimes w_2}_{V\otimes W}
      &= \IP{\Bigg(\sum\limits_{i=1}^nv_1^ie_i\Bigg)
                \otimes
              \Bigg(\sum\limits_{j=1}^mw_1^jf_j\Bigg)}
            {\Bigg(\sum\limits_{p=1}^nv_2^pe_p\Bigg)
                \otimes
              \Bigg(\sum\limits_{q=1}^mw_2^qf_q\Bigg)} 
            _{V\otimes W} \\
      &= \sum\limits_{i=1}^n
         \sum\limits_{j=1}^m
         \sum\limits_{p=1}^n
         \sum\limits_{q=1}^m
            v_1^iw_1^jv_2^pw_2^q\,
            \ip{e_i\otimes f_j}{e_p\otimes f_q}_{V\otimes W} \\
      &= \sum\limits_{i=1}^n
         \sum\limits_{j=1}^m
         \sum\limits_{p=1}^n
         \sum\limits_{q=1}^m
            v_1^iw_1^jv_2^pw_2^q\,
            \ip{e_i}{e_p}_V\ip{f_j}{f_q}_W \\
      &= \IP{\sum\limits_{i=1}^nv_1^ie_i}{\sum\limits_{p=1}^nv_2^pe_p}_V\cdot
         \IP{\sum\limits_{j=1}^mw_1^jf_j}{\sum\limits_{q=1}^mw_2^qf_q}_W \\
      &= \ip{v_1}{v_2}_V\cdot\ip{w_1}{w_2}_W \,.
    {{/latexeqn_}}
    
  {{/proof}}

{{/section}}

{{#section "secGrouping"}}

  {{#paragraph}}
    Again, we take a look at the tensor
  {{/paragraph}}

  {{#latexeqn_}}
    T =
    3\,e_1\otimes e_1\otimes e_1 +
    6\,e_1\otimes e_1\otimes e_2 +
    4\,e_2\otimes e_2\otimes e_1 +
    8\,e_2\otimes e_2\otimes e_2
    \in\R^2\otimes\R^2\otimes\R^2\,.
  {{/latexeqn_}}

  {{#paragraph}}
    Using associativity, let us assume, that we resolve the 
    right-hand side tensor product first, inducing the execution 
    order {{#latex}}\R^2\otimes(\R^2\otimes\R^2){{/latex}}.
    Now, we will consider the outcome of this first 
    tensor product as another vector space and rewrite 
    the second tensor product be means of that space.
    We define
  {{/paragraph}}

  {{#latexeqn "eqnRProdSpace"}}
    \R^{2\otimes 2}\def\R^2\otimes\R^2
  {{/latexeqn}}

  {{#paragraph}}
    and relabel its basis elements by
    {{#latex}}
        f_1\def e_1\otimes e_1,\,
        f_2\def e_1\otimes e_2,\,
        f_3\def e_2\otimes e_1,\,
        f_4\def e_2\otimes e_2
    {{/latex}}.
    Then {{#latex}}T{{/latex}} reads
  {{/paragraph}}

  {{#latexeqn_}}
    T =
    3\,e_1\otimes f_1 +
    6\,e_1\otimes f_2 +
    4\,e_2\otimes f_3 +
    8\,e_2\otimes f_4
    \in\R^2\otimes\R^{2\otimes 2}\,.
  {{/latexeqn_}}

  {{#paragraph}}
    By doing so, we have reduced the amount of indices in the description
    of {{#latex}}T{{/latex}} by merging index 2 and 3. The representation 
    has changed from (including a reshape of the coefficients data structure)
  {{/paragraph}}

  {{#latexeqn_}}
    T = 
    \sum\limits_{i=1}^2
    \sum\limits_{j=1}^2
    \sum\limits_{k=1}^2
    T^{ijk}\,e_i\otimes e_j\otimes e_k
    \in\R^2\otimes\R^2\otimes\R^2
  {{/latexeqn_}}

  {{imageHeight "tensors-order-3.svg" "150em"}}

  {{#paragraph}}
    to 
  {{/paragraph}}

  {{#latexeqn_}}
    T = 
    \sum\limits_{i=1}^2
    \sum\limits_{j=1}^4
    \tilde{T}^{ij}\,e_i\otimes f_j
    \in\R^2\otimes\R^{2\otimes 2}\,.
  {{/latexeqn_}}

  {{imageHeight "tensors-order-3-grouped.svg" "100em"}}

  {{#paragraph}}
    In general we can treat an arbitrary large tensor 
    product in this way. Since we consider the tensor 
    product to be commutative, we can assume 
    that the spaces to be merged (multiplied first) 
    are all located at the end of the tensor product.
  {{/paragraph}}

  {{#definition "defGrouping"}}
    
    {{#paragraph}}
      Let       
    {{/paragraph}}

    {{#latexeqn_}}
        T =
        \sum\limits_{i_1=1}^{n_1}\cdots\sum\limits_{i_s=1}^{n_s}
        T^{i_1\ldots i_s}\,e^{(1)}_{i_1}\otimes\ldots\otimes e^{(s)}_{i_s}
        \in V_1\otimes\ldots\otimes V_s
    {{/latexeqn_}}

    {{#paragraph}}
      be a tensor. For
      {{#latex}}W=V_{r+1}\otimes\ldots\otimes V_s,\,1\le r+1<s{{/latex}} 
      with basis {{#latex}}\{f_1,\ldots,f_m\}{{/latex}}, we call the operaton
      of describing {{#latex}}T{{/latex}} as
    {{/paragraph}}

    {{#latexeqn_}}
        T =
        \sum\limits_{i_1=1}^{n_1}\cdots\sum\limits_{i_r=1}^{n_r}
          \sum\limits_{j=1}^m
        \tilde{T}^{i_1\ldots i_rj}\,e^{(1)}_{i_1}\otimes\ldots\otimes e^{(r)}_{i_r}
          \otimes f_j
        \in V_1\otimes\ldots\otimes V_r\otimes W
    {{/latexeqn_}}

    {{#paragraph}}
      {{#highlight}}grouping{{/highlight}} of the indices 
      {{#latex}}i_{r+1},\ldots,i_s{{/latex}}. If we do the opposite and expand a 
      space of a tensor product into a product itself, we would call the 
      change of description of a tensor {{#highlight}}ungrouping{{/highlight}} of 
      the according index.
    {{/paragraph}}

  {{/definition}}

  {{#paragraph}}
    Note, that grouping/ungrouping changes the order of the tensor. Although being 
    the same tensor, its order is dependent on the indices used to label its coefficients.
  {{/paragraph}}

{{/section}}

{{#section "secContraction"}}

  {{#paragraph}}
    For the following we need quite a useful symbol to express 
    index equality.
  {{/paragraph}}

  {{#definition "defKronecker"}}

    {{#paragraph}}
      The {{#highlight}}Kronecker delta{{/highlight}} is a 
      symbol with two indices. To be universally applicable, 
      each index could be covariant (written subscript) or 
      contravariant (written superscript). We have
    {{/paragraph}}

    {{#latexeqn_}}
      \delta_{ij}=\delta^i_j=\delta_i^j=\delta^{ij}
      \def 
        \begin{cases}
          0,\quad i\neq j\,, \\
          1,\quad i=j\,.          
        \end{cases}   
    {{/latexeqn_}}

  {{/definition}}

  {{#paragraph}}
    Let {{#latex}}V{{/latex}} be a vector space over 
    {{#latex}}\F{{/latex}} with basis
    {{#latex}}\{e_1,\ldots,e_n\}{{/latex}}. Let 
    {{#latex}}V^\ast{{/latex}} be its dual space, and 
    {{#latex}}\{e^1,\ldots,e^n\}{{/latex}} the respective
    dual basis. 
    We recall that the dual space {{#latex}}V^\ast{{/latex}} 
    is in fact the space of all linear functionals on 
    {{#latex}}V{{/latex}}, i.e. 
    {{#latex}}V^\ast=\cL(V,\F){{/latex}}. Furthermore,
    the link between basis elements and dual 
    basis elements is the identity
  {{/paragraph}}

  {{#latexeqn_}}
    e^i(e_j)=\delta^i_j\,.
  {{/latexeqn_}}

  {{#paragraph}}
    Note, that all elements of {{#latex}}V^\ast{{/latex}} 
    are in fact linear functionals and can be applied to elements 
    of of {{#latex}}V{{/latex}}. 
    For tensors
  {{/paragraph}}

  {{#latexeqn_}}
    T = 
    \sum\limits_{j=1}^n 
    \sum\limits_{i=1}^n    
    T^j_i\,e_j\otimes e^i\in V\otimes V^\ast\,,
  {{/latexeqn_}}

  {{#paragraph}}
    the <em>contraction</em> of the indices 
    {{#latex}}i{{/latex}} and {{#latex}}j{{/latex}}
    is the map
  {{/paragraph}}

  {{#latexeqn "eqnContraction"}}
    & C_{i,j}:V\otimes V^*\longrightarrow\F,\\
    & C_{i,j}(T)\def
    \sum\limits_{j=1}^n 
    \sum\limits_{i=1}^n    
    T^j_i\,\underbrace{e^i(e_j)}_{=\delta^i_j}
    = \sum\limits_{k=1}^nT^k_k
    \,.
  {{/latexeqn}}

  {{#paragraph}}
    This term can be defined in general using the notions of 
    {{ref "tensors" "defTensorOrder"}}. A contraction will always
    contract a covariant index with a contravariant index.
    Due to commutativity we can assume without loss of generality, 
    that the indices to be contracted are the first ones of each group 
    (vector spaces or dual spaces, respectively).
  {{/paragraph}}

  {{#definition "defContraction"}}

    {{#paragraph}}
      Let
      {{#latex}}
        T\in W_1\otimes\ldots\otimes W_r\otimes V^\ast_1\otimes\ldots\otimes V^\ast_s
      {{/latex}}
      be a tensor with
    {{/paragraph}}

    {{#latexeqn_}}
      T =
      \sum\limits_{j_1=1}^{m_1}\cdots
      \sum\limits_{j_r=1}^{m_r}
      \sum\limits_{i_1=1}^{n_1}\cdots
      \sum\limits_{i_s=1}^{n_s}
      T^{j_1\ldots j_r}_{i_1\ldots i_s}\,
      f^{(1)}_{j_1}\otimes\ldots\otimes f^{(r)}_{j_r}\otimes
      e^{(1){i_1}}\otimes\ldots\otimes e^{(s){i_s}}
      \,.
    {{/latexeqn_}}

    {{#paragraph}}
      Let {{#latex}}W_1=V_1{{/latex}} be a matching vector space to dual space 
      {{#latex}}V^\ast_1{{/latex}}. Let
      {{#latex}}\{f^{(1)}_1,\ldots,f^{(1)}_{m_1}\}\subseteq W_1{{/latex}} be the dual basis 
      of {{#latex}}\{e^{(1)1},\ldots,e^{(1)n_1}\}\subseteq V^\ast_1{{/latex}}, 
      obviously {{#latex}}m_1=n_1\fed n{{/latex}}.
    {{/paragraph}}

    {{#paragraph}}
      The {{#highlight}}contraction{{/highlight}}
      of the indices {{#latex}}i_1{{/latex}} and
      {{#latex}}j_1{{/latex}} is the function
    {{/paragraph}}

    {{#latexeqn_}}
      C_{i_1,j_1}:
        W_1\otimes\ldots\otimes W_r\otimes V^\ast_1\otimes\ldots\otimes V^\ast_s
          \longrightarrow
        W_2\otimes\ldots\otimes W_r\otimes V^\ast_2\otimes\ldots\otimes V^\ast_s
    {{/latexeqn_}}

    {{#paragraph}}
      with
    {{/paragraph}}

    {{#latexeqn_}}
      C_{i_1,j_1}(T) 
      & \def
      \sum\limits_{j_1=1}^{m_1}\cdots
      \sum\limits_{j_r=1}^{m_r}
      \sum\limits_{i_1=1}^{n_1}\cdots
      \sum\limits_{i_s=1}^{n_s}
      T^{j_1\ldots j_r}_{i_1\ldots i_s}\cdot
      e^{(1)i_1}\Big(f^{(1)}_{j_1}\Big)\cdot
      f^{(2)}_{j_2}\otimes\ldots\otimes f^{(r)}_{j_r}\otimes
      e^{(2){i_2}}\otimes\ldots\otimes e^{(s){i_s}} \\
      & = 
      \sum\limits_{j_2=1}^{m_2}\cdots
      \sum\limits_{j_r=1}^{m_r}
      \sum\limits_{i_2=1}^{n_2}\cdots
      \sum\limits_{i_s=1}^{n_s}
      \Bigg(
        \underbrace{
          \sum\limits_{k=1}^n
          T^{kj_2\ldots j_r}_{ki_2\ldots i_s}
        }_{\fed\tilde{T}^{j_2\ldots j_r}_{i_2\ldots i_s}}
      \Bigg)\,
      f^{(2)}_{j_2}\otimes\ldots\otimes f^{(r)}_{j_r}\otimes
      e^{(2){i_2}}\otimes\ldots\otimes e^{(s){i_s}} \\
      & = 
      \sum\limits_{j_2=1}^{m_2}\cdots
      \sum\limits_{j_r=1}^{m_r}
      \sum\limits_{i_2=1}^{n_2}\cdots
      \sum\limits_{i_s=1}^{n_s}
      \tilde{T}^{j_2\ldots j_r}_{i_2\ldots i_s} \,
      f^{(2)}_{j_2}\otimes\ldots\otimes f^{(r)}_{j_r}\otimes
      e^{(2){i_2}}\otimes\ldots\otimes e^{(s){i_s}}
      \,.
    {{/latexeqn_}}

    {{#paragraph}}
      The case {{#latex}}r=s=1{{/latex}} is already defined in 
      equation {{ref "tensors" "eqnContraction"}}.
    {{/paragraph}}

  {{/definition}}

  {{#paragraph}}
    This definition seems to be rather complicated. For a tensor 
  {{/paragraph}}

  {{#latexeqn_}}
      T =      
      \sum\limits_{j=1}^n
      \sum\limits_{k=1}^m
      \sum\limits_{i=1}^n
      T^{jk}_i\,e_j\otimes f_k\otimes e^i 
      \in V\otimes W\otimes V^\ast
  {{/latexeqn_}}

  {{#paragraph}}
    let us illustrate the contraction of the indices 
    {{#latex}}i{{/latex}} and {{#latex}}j\,{{/latex}}:
  {{/paragraph}}

  {{#latexeqn_}}
      C_{i,j}(T) = \sum\limits_{k=1}^m\tilde{T}^k f_k\in W
  {{/latexeqn_}}

  {{#paragraph}}
    where
  {{/paragraph}}

  {{#latexeqn_}}
    \tilde{T}^k = \sum\limits_{i=1}^n T^{ik}_i\,.
  {{/latexeqn_}}

  {{imageHeight "tensors-contraction.svg" "240em"}}

  {{#paragraph}}
    The determination of a concrete example is straight forward.
  {{/paragraph}}

  {{#example "exContraction"}}

    {{#paragraph}}
      We consider the tensor
      {{#latex}}T\in\R^2\otimes\R^2\otimes\R^2{{/latex}}
      (we recall {{#latex}}(\R^2)^\ast=\R^2{{/latex}}),
      specified by
    {{/paragraph}}

    {{#latexeqn_}}
      T 
      & =      
      \sum\limits_{j=1}^n
      \sum\limits_{k=1}^m
      \sum\limits_{i=1}^n
      T^{jk}_i\,e_j\otimes e_k\otimes e^i \\
      & = 
      3\,e_1\otimes e_1\otimes e^1 +
      6\,e_1\otimes e_1\otimes e^2 +
      4\,e_2\otimes e_2\otimes e^1 +
      8\,e_2\otimes e_2\otimes e^2
      \,.      
    {{/latexeqn_}}

    {{#paragraph}}
      We calculate
    {{/paragraph}}

    {{#latexeqn_}}
      C_{i,j}(T) 
      & =
      3\,\underbrace{e^1(e_1)}_{=1}\,e_1 +
      6\,\underbrace{e^2(e_1)}_{=0}\,e_1 +
      4\,\underbrace{e^1(e_2)}_{=0}\,e_2 +
      8\,\underbrace{e^2(e_2)}_{=1}\,e_2 \\
      & =
      3e_1+8e_2\in\R^2\,.
    {{/latexeqn_}}

    {{imageHeight "tensors-contraction-example.svg" "150em"}}

  {{/example}}  

  {{#paragraph}}
    Since always a covariant index and a contravariant index will 
    be contracted, an order-{{#latex}}(r,s){{/latex}} tensor will be turned 
    into an order-{{#latex}}(r-1,s-1){{/latex}} tensor. Directly from 
    definiton we obtain further results.
  {{/paragraph}}

  {{#theorem "obsContractionLinearity"}}

    {{#paragraph}}
      The contraction is a linear function, i.e. for tensors 
      {{#latex}}T,T_1,T_2{{/latex}} from a tensor space over 
      {{#latex}}\F{{/latex}} and {{#latex}}a\in\F{{/latex}}
      we have
    {{/paragraph}}

    {{#latexeqn_}}
      & C_{i,j}(a\cdot T) = a\cdot C_{i,j}(T)\,, \\
      & C_{i,j}(T_1+T_2) = C_{i,j}(T_1) + C_{i,j}(T_2)\,.
    {{/latexeqn_}}

  {{/theorem}}

  {{#proof}}

    {{#paragraph}}
      Both equations can be validated directly from 
      {{ref "tensors" "defContraction"}}.
    {{/paragraph}}

  {{/proof}}

  {{#theorem "obsContractionOrder"}}

    {{#paragraph}}
      If we contract a tensor {{#latex}}T{{/latex}} twice,
      then the order of both contractions does not matter. 
      We have for covariant indices {{#latex}}i_1,i_2{{/latex}}
      and contravariant indices {{#latex}}j_1,j_2{{/latex}}
      the identity
    {{/paragraph}}

    {{#latexeqn_}}
      C_{i_1,j_1}(C_{i_2,j_2}(T)) 
      = C_{i_2,j_2}(C_{i_1,j_1}(T))
      \,.
    {{/latexeqn_}}

  {{/theorem}}

  {{#proof}}

    {{#paragraph}}
      The assertion results from direct application of 
     {{ref "tensors" "defContraction"}}. 
    {{/paragraph}}

  {{/proof}}

  {{#theorem "obsContractionProduct"}}

    {{#paragraph}}
      The contraction is interchangable with the tensor product.
      Let {{#latex}}i{{/latex}} be a covariant index of a 
      tensor {{#latex}}T_1{{/latex}} and {{#latex}}j{{/latex}} 
      be a contravariant index of {{#latex}}T_1{{/latex}}.
      Contracting {{#latex}}i{{/latex}} and {{#latex}}j{{/latex}} 
      can be executed before or after a tensor product with another tensor, 
      the result is the same.
    {{/paragraph}}

    {{#latexeqn_}}
      C_{i,j}(T_1)\otimes T_2 = C_{i,j}(T_1\otimes T_2)
    {{/latexeqn_}}

  {{/theorem}}

  {{#proof}}

    {{#paragraph}}
      The equation is derived directly from definitions 
      {{ref "tensors" "defProduct"}} and {{ref "tensors" "defContraction"}}.
    {{/paragraph}}

  {{/proof}}

  {{#paragraph}}
    Contracting consecutively all indices of a tensor will always lead to 
    a scalar result. The simplest case is the contraction of a matrix.
  {{/paragraph}}

  {{#example "exMatrixContraction"}}

    {{#paragraph}}
      We recall equation {{ref "tensors" "eqnContraction"}},
      {{#latex}}
        C_{i,j}:V\otimes V^*\longrightarrow\F,\,
        C_{i,j}(T) = \sum\limits_{k=1}^nT^k_k\,
      {{/latex}}.
      Considering the coefficients of {{#latex}}T{{/latex}} as matrix,
      we obtain
    {{/paragraph}}

    {{#latexeqn_}}
      C_{i,j}(T)=\tr(T)\,.
    {{/latexeqn_}}

  {{/example}}

{{/section}}

{{#section "secMultilinear"}}

  {{#paragraph}}
    Having introduced the main tensor operations - tensor product, grouping,
    contraction - we are able to generalize more building blocks of
    Linear Algebra. We start with linear maps.
    Let {{#latex}}V,W{{/latex}} be vector spaces, 
    let {{#latex}}\{e_1,\ldots,e_n\}{{/latex}} be a basis of
    {{#latex}}V{{/latex}} and let {{#latex}}\{f_1,\ldots,f_m\}{{/latex}} 
    be a basis of {{#latex}}W{{/latex}}. We recall, how the matrix
    representation of a linear map {{#latex}}S\in\cL(V,W){{/latex}}
    is derived. For {{#latex}}v=\sum\limits_{i=1}^nv^ie_i\in V{{/latex}} 
    and {{#latex}}S(e_i)=\sum\limits_{j=1}^mS^j_if_j\in W{{/latex}} 
    we compute
  {{/paragraph}}

  {{#latexeqn "eqnLinearMap"}}
    S(v) 
    &= S\Bigg(\sum\limits_{i=1}^nv^ie_i\Bigg) \\
    &= \sum\limits_{i=1}^nv^iS(e_i) \\
    &= \sum\limits_{i=1}^nv^i\sum\limits_{j=1}^mS^j_if_j \\
    &= \sum\limits_{j=1}^m\sum\limits_{i=1}^nS^j_iv^if_j \,.
  {{/latexeqn}}

  {{#paragraph}}
    Let {{#latex}}V^\ast{{/latex}} be the dual space of {{#latex}}V{{/latex}},
    and let {{#latex}}\{e^1,\ldots,e^n\}{{/latex}} be the dual basis. Let 
    {{#latex}}
      T=\sum\limits_{j=1}^m\sum\limits_{i=1}^nT^j_i\,f_j\otimes e^i
      \in W\otimes V^\ast
    {{/latex}}
    be a tensor. For
    {{#latex}}v=\sum\limits_{k=1}^nv^ke_k\in V{{/latex}} we define 
    a map {{#latex}}T':V\rightarrow W{{/latex}} by
  {{/paragraph}}

  {{#latexeqn "eqnContractionMap"}}
    T'(v)\def C_{i,k}(T\otimes v)\,.
  {{/latexeqn}}

  {{#paragraph}}
    We recall bilinearity of the tensor product ({{ref "tensors" "defProduct"}}) 
    and linearity of the contraction ({{ref "tensors" "obsContractionLinearity"}}).
    Together both imply that {{#latex}}T'{{/latex}} is linear as well, i.e. 
    {{#latex}}T'\in\cL(V,W){{/latex}}. Its matrix representation can be obtained 
    by resolving the contraction.
  {{/paragraph}}

  {{#latexeqn "eqnLinearMapContraction"}}
    T'(v) &= C_{i,k}(T\otimes v) \\
    &= C_{i,k}\Bigg(\Bigg[
      \sum\limits_{j=1}^m
      \sum\limits_{i=1}^n
      T^j_i\,f_j\otimes e^i\Bigg]
      \otimes\Bigg[
      \sum\limits_{k=1}^n
      v^ke_k      
      \Bigg]\Bigg) \\
    &= C_{i,k}\Bigg(
      \sum\limits_{j=1}^m
      \sum\limits_{i=1}^n
      \sum\limits_{k=1}^n
      T^j_iv^k\,f_j\otimes e^i\otimes e_k      
      \Bigg) \\
    &= C_{i,k}\Bigg(
      \sum\limits_{j=1}^m
      \sum\limits_{i=1}^n
      \sum\limits_{k=1}^n
      T^j_iv^k\,f_j\otimes\underbrace{e^i(e_k)}_{=\delta^i_k}      
      \Bigg) \\
    &= \sum\limits_{j=1}^m\sum\limits_{i=1}^nT^j_iv^if_j \,.
  {{/latexeqn}}

  {{#paragraph}}
    Comparing {{ref "tensors" "eqnLinearMap"}} and 
    {{ref "tensors" "eqnLinearMapContraction"}} we obtain 
    that the coefficients {{#latex}}T^j_i{{/latex}} 
    of tensor {{#latex}}T{{/latex}} form the matrix representation 
    of the linear map {{#latex}}T'{{/latex}}. This twofold interpretation of 
    {{#latex}}T^j_i{{/latex}} as tensor coefficients and matrix of a 
    linear map provides the natural isomorphism 
    {{#latex}}W\otimes V^\ast\simeq\cL(V,W){{/latex}}.
    Nevertheless, to be precise during theory development, 
    we will continue to differentiate linear map and related tensor
    by adding a prime to the character denoting the map. Later we will abandon 
    this distinction.  After having discussed the case of a expressing 
    a linear map with a tensor, we will take this approach even further 
    and lift it to multilinear maps.
  {{/paragraph}}

  {{#theorem "thmMultilinearity"}}

    {{#paragraph}}
      Let {{#latex}}V_1,\ldots,V_s{{/latex}} be vector spaces with bases
      {{#latex}}\{e^{(i)}_1,\ldots,e^{(i)}_{n_i}\}{{/latex}} and 
      respective dual bases
      {{#latex}}\{e^{(i)1},\ldots,e^{(i)n_i}\}{{/latex}}. In addition 
      let {{#latex}}W{{/latex}} be a vector space with basis 
      {{#latex}}\{f_1,\ldots,f_m\}.{{/latex}}
      Let 
    {{/paragraph}}

    {{#latexeqn_}}
      T =
      \sum\limits_{j=1}^{m}
      \sum\limits_{i_1=1}^{n_1}\cdots
      \sum\limits_{i_s=1}^{n_s}
      T^j_{i_1\ldots i_s}\,
      f_j\otimes e^{(1){i_1}}\otimes\ldots\otimes e^{(s){i_s}}
    {{/latexeqn_}}

    {{#paragraph}}
      be a tensor of 
      {{#latex}}W\otimes V^\ast_1\otimes\ldots\otimes V^\ast_s{{/latex}}.
      Then we can define a multilinear map 
      {{#latex}}T'\in\cL^s(V_1,\ldots,V_s \, ; \, W){{/latex}}
      as follows. For 
      {{#latex}}
        v_p
        = \sum\limits_{k_p=1}^{n_p}v_p^{k_p}e^{(p)}_{k_p}
        \in V_p
      {{/latex}}
      we set
    {{/paragraph}}

    {{#latexeqn_}}
      T'(v_1,\ldots,v_s)
      \def
      (C_{i_1,k_1}\circ\ldots\circ C_{i_s,k_s})
        (T\otimes v_1\otimes\ldots\otimes v_s)\,.
    {{/latexeqn_}}

    {{#paragraph}}
      The way tensor {{#latex}}T{{/latex}} is used to define 
      {{#latex}}T'{{/latex}} provides an isomorphism
    {{/paragraph}}

    {{#latexeqn_}}
      W\otimes V^\ast_1\otimes\ldots\otimes V^\ast_s
      \simeq
      \cL^s(V_1,\ldots,V_s \, ; \, W) \,.
    {{/latexeqn_}}

  {{/theorem}}

  {{#sketch}}

    {{#paragraph}}
      The argument follows the same path as already explained for the
      case {{#latex}}s=1{{/latex}}. Bilinearity of the tensor product 
      ensures that 
    {{/paragraph}}

    {{#latexeqn_}}
      (v_1,\ldots,v_s)
      \longmapsto 
      T\otimes v_1\otimes\ldots\otimes v_s
    {{/latexeqn_}}

    {{#paragraph}}
      is linear in each variable. Linearity of contractions preserves that.
      Hence {{#latex}}T'{{/latex}} is linear in each variable and therefore 
    {{/paragraph}}

    {{#latexeqn_}}
      T'\in\cL^s(V_1,\ldots,V_s \, ; \, W) \,.
    {{/latexeqn_}}

    {{#paragraph}}
      Calculating
    {{/paragraph}}
    
    {{#latexeqn_}}
      T'(v_1,\ldots,v_s) 
      &=
      (C_{i_1,k_1}\circ\ldots\circ C_{i_s,k_s})
        (T\otimes v_1\otimes\ldots\otimes v_s) \\
      &=\sum\limits_{j=1}^{m}
        \sum\limits_{i_1=1}^{n_1}\cdots
        \sum\limits_{i_s=1}^{n_s}
        T^j_{i_1\ldots i_s}v_1^{i_1}\cdots v_s^{i_s}f_j
      \,.
    {{/latexeqn_}}
    
    {{#paragraph}}
      yields {{#latex}}T^j_{i_1\ldots i_s}v_1^{i_1}\cdots v_s^{i_s}{{/latex}}
      as coefficients. If one strips down a multilinear map 
      {{#latex}}S\in\cL^s(V_1,\ldots,V_s \, ; \, W){{/latex}} to 
      its effects on basis elements - in a similar way linear maps are 
      expressed by means of their matrix representation - then one 
      obtains the same coefficients 
      {{#latex}}S^j_{i_1\ldots i_s}v_1^{i_1}\cdots v_s^{i_s}{{/latex}}.
      This delivers an isomorphism for 
    {{/paragraph}}

    {{#latexeqn_}}
      W\otimes V^\ast_1\otimes\ldots\otimes V^\ast_s
      \simeq
      \cL^s(V_1,\ldots,V_s \, ; \, W) \,.
    {{/latexeqn_}}

  {{/sketch}}

  {{#paragraph}}
    We will give a few examples to illustrate how a linear transformation 
    can be realized by a tensor.
  {{/paragraph}}

  {{#example "exHadamard"}}

    {{#paragraph}}
      Let
    {{/paragraph}}

    {{#latexeqn_}}
      H=
      \begin{bmatrix}
        1 & 1 \\
        1 & -1
      \end{bmatrix}
    {{/latexeqn_}}

    {{#paragraph}}
      be the Hadamard matrix of order 2. Let {{#latex}}H'{{/latex}} be the associated 
      linear map of {{#latex}}\cL(\R^2){{/latex}} - with basis of 
      {{#latex}}\R^2{{/latex}} being the standard basis 
      {{#latex}}\{e_1\def [1\;\;0]^T,\,e_2\def [0\;\;1]^T\}{{/latex}} for 
      domain space as well as for image space. It is folklore to obtain the 
      image of {{#latex}}v=ae_1+be_2{{/latex}} by feeding 
      {{#latex}}[a\;\;b]^T{{/latex}} into
    {{/paragraph}}

    {{#latexeqn_}}
      \begin{bmatrix}
        1 & 1 \\
        1 & -1
      \end{bmatrix}
      \begin{bmatrix}
        a \\
        b
      \end{bmatrix}
      =
      \begin{bmatrix}
        a+b \\
        a-b
      \end{bmatrix}
    {{/latexeqn_}}

    {{#paragraph}}
      to obtain {{#latex}}H'(v)=(a+b)e_1+(a-b)e_2{{/latex}}.
    {{/paragraph}}

    {{#paragraph}}
      We want to do the same exercise using {{#latex}}H{{/latex}} as tensor.
      We can rephrase {{#latex}}H{{/latex}} (using the same coefficients) as
    {{/paragraph}}    

    {{#latexeqn_}}
      H =
      e_1\otimes e^1 +
      e_1\otimes e^2 +
      e_2\otimes e^1 -
      e_2\otimes e^2 \,.
    {{/latexeqn_}}

    {{#paragraph}}
      Let {{#latex}}i{{/latex}} be the covariant index of {{#latex}}H{{/latex}} 
      and {{#latex}}k{{/latex}} be the index of the argument {{#latex}}v{{/latex}}.
      We can express application of {{#latex}}H'{{/latex}} as
    {{/paragraph}}

    {{#latexeqn_}}
      H'(v) &= C_{i,k}(H\otimes v) \\
      &= C_{i,k}(
        (e_1\otimes e^1 +
        e_1\otimes e^2 +
        e_2\otimes e^1 -
        e_2\otimes e^2)
        \otimes
        (ae_1+be_2)
      ) \\
      &= C_{i,k}(
        ((e_1+e_2)\otimes e^1 + (e_1-e_2)\otimes e^2) 
        \otimes
        (ae_1+be_2)
      ) \\
      &= a(e_1+e_2)\underbrace{e^1(e_1)}_{=1}
        + b(e_1+e_2)\underbrace{e^1(e_2)}_{=0}
        + a(e_1-e_2)\underbrace{e^2(e_1)}_{=0}        
        + b(e_1-e_2)\underbrace{e^2(e_2)}_{=1} \\
      &= a(e_1+e_2) + b(e_1-e_2) \\
      &= (a+b)e_1+(a-b)e_2\,.
    {{/latexeqn_}}

    {{#paragraph}}
      This might appear clumsy on first sight. But this approach will later turn out 
      to be very powerful, especially when the linear map is interacting between 
      tensor product spaces.
    {{/paragraph}}

  {{/example}}

  {{#example "exIdentity"}}

    {{#paragraph}}
      The identity map {{#latex}}I'\in\cL(\R^2){{/latex}} can be realized by tensor
    {{/paragraph}}

    {{#latexeqn_}}
      I=e_1\otimes e^1+e_2\otimes e^2\,.
    {{/latexeqn_}}

    {{#paragraph}}
      Although this is true regardless of the chosen basis, 
      {{#latex}}\{e_1,e_2\}{{/latex}} shall still denote the standard basis. 
      To validate that linear maps match, it is sufficient to check all basis arguments.
      So, let us outline where the {{#latex}}I{{/latex}} induced map sends
      {{#latex}}e_1{{/latex}} and {{#latex}}e_2{{/latex}}.
    {{/paragraph}}

    {{#latexeqn_}}
      & e_1 \longmapsto 
        e_1\otimes\underbrace{e^1(e_1)}_{=1}
        + e_2\otimes\underbrace{e^2(e_1)}_{=0}
        = e_1\,, \\
      & e_2 \longmapsto 
        e_1\otimes\underbrace{e^1(e_2)}_{=0}
        + e_2\otimes\underbrace{e^2(e_2)}_{=1}
        = e_2\,. \\
    {{/latexeqn_}}

    {{#paragraph}}
      Indeed, this is the identity.
    {{/paragraph}}

  {{/example}}

  {{#example "exNot"}}

    {{#paragraph}}
      The Hadamard transformation (and the identity of course as well) will play 
      a prominent role in the later Quantum Computing elaboration. The NOT gate is 
      another building block. A quantum bit (<em>qubit</em>) holds state values of 
      {{#latex}}\C^2{{/latex}}. For the sake of simplicity we will stick to 
      {{#latex}}\R^2{{/latex}} here. Again, let {{#latex}}\{e_1,e_2\}{{/latex}} be
      the standard basis. {{#latex}}e_1{{/latex}} plays the role of logical 
      <em>false</em>, {{#latex}}e_2{{/latex}} features logical <em>true</em>. A 
      linear combindation of both reflects the qubit being in superposition.
    {{/paragraph}}

    {{#paragraph}}
      The NOT gate {{#latex}}X'{{/latex}} negates the logical values 
      {{#latex}}e_1{{/latex}} and {{#latex}}e_2{{/latex}}, i.e.
    {{/paragraph}}

    {{#latexeqn_}}
      X'(e_1)=e_2\,,\\
      X'(e_2)=e_1\,. 
    {{/latexeqn_}}

    {{#paragraph}}
      Compared to the identity map, we have to cross-wire {{#latex}}e_1{{/latex}} 
      with {{#latex}}e^2{{/latex}} and vice versa. The tensor realizing
      {{#latex}}X'{{/latex}} is
    {{/paragraph}}

    {{#latexeqn_}}
      X=e_1\otimes e^2+e_2\otimes e^1\,.
    {{/latexeqn_}}

    {{#paragraph}}
      Indeed, we can verify
    {{/paragraph}}

    {{#latexeqn_}}
      & e_1 \longmapsto 
        e_1\otimes\underbrace{e^2(e_1)}_{=0}
        + e_2\otimes\underbrace{e^1(e_1)}_{=1}
        = e_2\,, \\
      & e_2 \longmapsto 
        e_1\otimes\underbrace{e^2(e_2)}_{=1}
        + e_2\otimes\underbrace{e^1(e_2)}_{=0}
        = e_1\,. \\
    {{/latexeqn_}}
    
  {{/example}}

  {{#paragraph}}
    We succeeded in expressing any linear transformation between 
    vector spaces as a contraction of a tensor product. How does this 
    apply to chaining of linear transformations? Let 
    {{#latex}}T'\in\cL(V,W){{/latex}} and {{#latex}}S'\in\cL(W,X){{/latex}}.
    We have learned that both linear functions can also be constructed
    with tensors having same coefficients (as the matrix representations) 
    in same bases. Let    
  {{/paragraph}}

  {{#latexeqn_}}
      T &=
        \sum\limits_{j=1}^m\sum\limits_{i=1}^nT^j_i\,f_j\otimes e^i
        \in W\otimes V^\ast \,, \\
      S &=
        \sum\limits_{q=1}^r\sum\limits_{p=1}^mS^q_p\,g_q\otimes f^p
        \in X\otimes W^\ast
  {{/latexeqn_}}

  {{#paragraph}}
    denote these tensors. Take notice of 
    {{#latex}}\{g_1,\ldots,g_r\}{{/latex}} being basis of 
    {{#latex}}X{{/latex}}, all other already introduced notations 
    remain - same basis characters  with superscript and subscript 
    indices form dual bases.
  {{/paragraph}}

  {{#theorem "thmComposition"}}

    {{#paragraph}}
      Composition of linear maps can be expressed by contraction and 
      tensor product. The tensor associated to the map 
      {{#latex}}S'\circ T'\in\cL(V,X){{/latex}} is 
    {{/paragraph}}

    {{#latexeqn_}}
      C_{p,j}(S\otimes T)
      =
      \sum\limits_{q=1}^r\sum\limits_{i=1}^n\sum\limits_{j=1}^m
      S^q_jT^j_i\,g_q\otimes e^i
      \in X\otimes V^\ast \,.
    {{/latexeqn_}}

    {{#paragraph}}
      For {{#latex}}v=\sum\limits_{k=1}^nv^ke_k\in V{{/latex}} the 
      map is realized by
    {{/paragraph}}

    {{#latexeqn_}}
      (S'\circ T')(v) = C_{i,k}(C_{p,j}(S\otimes T)\otimes v)\,.
    {{/latexeqn_}}

  {{/theorem}}

  {{#proof}}

    {{#paragraph}}
      Using {{ref "tensors" "obsContractionOrder"}},
      {{ref "tensors" "obsContractionProduct"}} and
      {{ref "tensors" "thmMultilinearity"}}
      we can transform
    {{/paragraph}}

    {{#latexeqn_}}
      (S'\circ T')(v)
      &= S'(C_{i,k}(T\otimes v)) \\
      &= C_{p,j}(S\otimes C_{i,k}(T\otimes v)) \\
      &= C_{p,j}(C_{i,k}(S\otimes T\otimes v)) \\
      &= C_{i,k}(C_{p,j}(S\otimes T\otimes v)) \\
      &= C_{i,k}(C_{p,j}(S\otimes T)\otimes v)
      \,.
    {{/latexeqn_}}

  {{/proof}}

  {{#paragraph}}
    The possibility to define linear maps with help of tensors will
    help us now to extend linear maps to tensor product spaces.
    Let {{#latex}}T'\in\cL(V,W){{/latex}} and 
    {{#latex}}S'\in\cL(X,Y){{/latex}}. In addition to notations already in use, 
    let {{#latex}}\{h_1,\ldots,h_s\}{{/latex}} denote a basis of 
    {{#latex}}Y{{/latex}}. Let {{#latex}}T{{/latex}} and {{#latex}}S{{/latex}}
    be the tensors realizing {{#latex}}T'{{/latex}} and {{#latex}}S'{{/latex}}.
    Then for {{#latex}}v=\sum\limits_{k=1}^nv^ke_k\in V{{/latex}} and 
    {{#latex}}x=\sum\limits_{l=1}^rx^lg_l\in X{{/latex}}, we have
  {{/paragraph}}

  {{#latexeqn_}}
    T &=
        \sum\limits_{j=1}^m\sum\limits_{i=1}^nT^j_i\,f_j\otimes e^i
        \in W\otimes V^\ast \,, \\
    T'(v) &= C_{i,k}(T\otimes v) \,, \\
    S &=
      \sum\limits_{q=1}^s\sum\limits_{p=1}^rS^q_p\,h_q\otimes g^p
      \in Y\otimes X^\ast \,, \\
    S'(x) &= C_{p,l}(S\otimes x) \,.
  {{/latexeqn_}}

  {{#paragraph}}
    We are going to combine {{#latex}}T'{{/latex}} and {{#latex}}S'{{/latex}}
    in a natural way to obtain a linear map 
    {{#latex}}T'\odot S'\in\cL(V\otimes X,W\otimes Y){{/latex}}. We will do 
    this with help of
  {{/paragraph}}

  {{#latexeqn "eqnProductMap"}}
    T\otimes S
    &=
    \sum\limits_{j=1}^m
    \sum\limits_{i=1}^n
    \sum\limits_{q=1}^s
    \sum\limits_{p=1}^r
    T^j_iS^q_p \,
    f_j\otimes e^i\otimes h_q\otimes g^p
    \in W\otimes V^\ast\otimes Y\otimes X^\ast
    \,.
  {{/latexeqn}}

  {{#paragraph}}
    One strategy to define {{#latex}}T'\odot S'{{/latex}}
    would be to group indices {{#latex}}i,p{{/latex}} and 
    {{#latex}}j,q{{/latex}} in {{#latex}}T\otimes S{{/latex}} and then 
    follow {{ref "tensors" "thmMultilinearity"}}. However, this would 
    require to justify that {{#latex}}V^\ast\otimes X^\ast{{/latex}}
    (part of definition of {{#latex}}T\otimes S{{/latex}}) can be 
    identified with {{#latex}}(V\otimes X)^\ast{{/latex}} 
    (necessary to define the linear map). We will deviate from this 
    strategy and use the fact that the tensor is given as tensor product.
    We will use {{#latex}}T{{/latex}} and {{#latex}}S{{/latex}} separately 
    to define the map for arguments from {{#latex}}V\otimes X{{/latex}} 
    that are <em>separable</em>. Afterwards we will continue the function 
    linearly to all possible arguments.
  {{/paragraph}}

  {{#theorem "lemProductMap"}}

    {{#paragraph}}
      We define {{#latex}}T'\odot S':V\otimes X\rightarrow W\otimes Y{{/latex}}
      by
    {{/paragraph}}

    {{#latexeqn_}}
      (T'\odot S')(u)
      \def 
      \begin{cases}
      C_{i,k}(C_{p,l}(T\otimes S\otimes v\otimes x)) \,,\quad 
        \text{for }u=v\otimes x,\,v\in V,\,x\in X, \\
      \sum\limits_{k=1}^n
      \sum\limits_{l=1}^r
      u^{kl}\,(T'\odot S')(e_k\otimes g_l) \,,\quad\text{otherwise.}
      \end{cases}
    {{/latexeqn_}}

    {{#paragraph}}
      Then {{#latex}}T'\odot S'{{/latex}} is well-defined and 
      {{#latex}}T'\odot S'\in\cL(V\otimes X,W\otimes Y){{/latex}}. 
      Furthermore, for {{#latex}}v\otimes x\in V\otimes X{{/latex}}
      holds
    {{/paragraph}}

    {{#latexeqn_}}
      (T'\odot S')(v\otimes x)=T'(v)\otimes S'(x)\,.
    {{/latexeqn_}}

  {{/theorem}}

  {{#proof}}

    {{#paragraph}}
      We start with the case that {{#latex}}u{{/latex}} is 
      <em>separable</em>, {{#latex}}u=v\otimes x{{/latex}}.
      Linearity follows again from linearity of 
      tensor product and contraction. We calculate
      (using {{ref "tensors" "obsContractionProduct"}})
    {{/paragraph}}
    
    {{#latexeqn_}}
      (T'\odot S')(v\otimes x)
      &= C_{i,k}(C_{p,l}(T\otimes S\otimes v\otimes x)) \\
      &= C_{i,k}(T\otimes v\otimes C_{p,l}(S\otimes x)) \\
      &= C_{i,k}(T\otimes v)\otimes C_{p,l}(S\otimes x) \\
      &= T'(v)\otimes S'(x)
      \in W\otimes Y \,.
    {{/latexeqn_}}

    {{#paragraph}}
      Now consider the case that {{#latex}}u{{/latex}} 
      is <em>entangled</em>. The basis representation       
    {{/paragraph}}

    {{#latexeqn_}}
      u =
      \sum\limits_{k=1}^n
      \sum\limits_{l=1}^r
      u^{kl}\,e_k\otimes g_l
    {{/latexeqn_}}

    {{#paragraph}}
      is unique. Hence,   
    {{/paragraph}}

    {{#latexeqn_}}
      (T'\odot S')(u)
      =
      \sum\limits_{k=1}^n
      \sum\limits_{l=1}^r
      u^{kl}\,(T'\odot S')(e_k\otimes g_l)
    {{/latexeqn_}}

    {{#paragraph}}
      is well-defined. It is also compatible with <em>separable</em> arguments
      (the function value would be the same also when fed into the second 
      line of the function definition) 
      and linear. Since the function value is 
      a linear combination of already defined values, it stays in 
      {{#latex}}W\otimes Y{{/latex}}. All in all, 
      {{#latex}}T'\odot S'{{/latex}} is 
      a well defined linear map fulfilling
      {{#latex}}T'\odot S'\in\cL(V\otimes X,W\otimes Y){{/latex}}.
    {{/paragraph}}

  {{/proof}}

  {{#paragraph}}
    The coefficients of {{#latex}}T\otimes S{{/latex}} are 
    {{#latex}}T^j_iS^q_p{{/latex}} (confer equation 
    {{ref "tensors" "eqnProductMap"}}). {{#latex}}T^j_i{{/latex}} 
    also represents the matrix of {{#latex}}T'{{/latex}} (in same bases),
    {{#latex}}S^q_p{{/latex}} represents the matrix of {{#latex}}S'{{/latex}}.
    This means, if we interpret {{#latex}}T'{{/latex}} and {{#latex}}S'{{/latex}}
    as vectors of vector spaces {{#latex}}\cL(V,W){{/latex}} and 
    {{#latex}}\cL(X,Y){{/latex}} respectively. Then {{#latex}}T^j_i,\,S^q_p{{/latex}} 
    would be their coefficients. Hence, {{#latex}}T^j_iS^q_p{{/latex}} would 
    be the coefficients of {{#latex}}T'\otimes S'\in\cL(V,W)\otimes\cL(X,Y){{/latex}}.
    This justifies to use {{#latex}}T'\otimes S'=T'\odot S'{{/latex}} when referring 
    to the composed linear map. Using this notation we express the results of
    {{ref "tensors" "lemProductMap"}} entirely by means of {{#latex}}T'{{/latex}}
    and {{#latex}}S'{{/latex}}.
  {{/paragraph}}

  {{#theorem "thmProductMap"}}

    {{#paragraph}}
      Let {{#latex}}T'\in\cL(V,W),\,S'\in\cL(X,Y){{/latex}}. Let 
      {{#latex}}T'\otimes S'\in\cL(V\otimes X,W\otimes Y){{/latex}} be 
      defined as mentioned before.
    {{/paragraph}}

    {{#paragraph}}
      If {{#latex}}u\in V\otimes X{{/latex}} is separable, i.e. 
      {{#latex}}u=v\otimes x{{/latex}}, then
    {{/paragraph}}

    {{#latexeqn_}}
      (T'\otimes S')(v\otimes x)=T'(v)\otimes S'(x) \,.
    {{/latexeqn_}}

    {{#paragraph}}
      For all (other) arguments the function value of 
      {{#latex}}
        u=
        \sum\limits_{k=1}^n
        \sum\limits_{l=1}^r
        u^{kl} \,
        e_k\otimes g_l
        \in V\otimes W 
      {{/latex}}
      is obtained from 
    {{/paragraph}}

    {{#latexeqn_}}
      (T'\otimes S')(u)=
      \sum\limits_{k=1}^n
      \sum\limits_{l=1}^r
      u^{kl} \,
      T'(e_k)\otimes S'(g_l) \,.
    {{/latexeqn_}}

  {{/theorem}}

  {{#paragraph}}
    Note, that this theorem (and the lemma before) makes heavy use of the fact, 
    that the effect of the linear map on {{#latex}}V\otimes X{{/latex}} 
    can be broken down to local transformations acting solely on 
    {{#latex}}V{{/latex}} or on {{#latex}}X{{/latex}}. Not all linear maps in
    {{#latex}}\cL(V\otimes W,X\otimes Y){{/latex}} are decompossible this way. 
    This is the case if and only if the linear map - considered as tensor - is 
    separable. We will see a separable as well as an entangled 
    linear map in the following examples. From now on, we will abandon the 
    strict distinction between multilinear map and generating tensor. We will 
    treat them as synonyms now and let them share the same character as identifier.
  {{/paragraph}}

  {{#example "exHadamardGate"}}

    {{#paragraph}}
      Based on {{ref "tensors" "exHadamard"}} we will extend the
      Hadamard transformation {{#latex}}H{{/latex}}
      into a quantum gate acting on two qubits. A two-qubit system is a state space 
      in {{#latex}}\C^2\otimes\C^2{{/latex}}. We will continue to simplify and 
      let the Hadamard gate act on {{#latex}}\R^2\otimes\R^2{{/latex}}, where 
      {{#latex}}\R^2{{/latex}} is equipped with standard basis 
      {{#latex}}\{e_1,e_2\}{{/latex}}. We will also neglect the fact that quantum states 
      are actually vectors of norm {{#latex}}1{{/latex}}, 
      which would enforce the Hadamard transformation to 
      contain a scaling factor of {{#latex}}1/\sqrt{2}{{/latex}}.
    {{/paragraph}}

    {{#paragraph}}
      We require the quantum gate to act on the 
      first qubit as Hadamard transformation and to leave the second qubit unchanged.
      {{ref "tensors" "thmProductMap"}} shows how we can combine these separate actions 
      on single qubits into a combined action on the two-qubit system - by merging 
      them by tensor product. The combined quantum gate is
    {{/paragraph}}

    {{#latexeqn_}}
      H\otimes I\in\R^2\otimes\R^2\,.
    {{/latexeqn_}}

    {{#paragraph}}
      Quantum circuits are usually initialized by setting all qubits to 
      logical false. In our example that would be 
      {{#latex}}e_1\otimes e_1{{/latex}}. We will calculate the result 
      of applying the just constructed quantum gate to this configuration:
    {{/paragraph}}

    {{#latexeqn_}}
      (H\otimes I)(e_1\otimes e_1) 
      &= H(e_1)\otimes I(e_1) \\
      &= (e_1+e_2)\otimes e_1 \\
      &= e_1 \otimes e_1+e_2\otimes e_1
      \,.
    {{/latexeqn_}}

  {{/example}}

  {{#example "exCnot"}}

    {{#paragraph}}
      The CNOT gate is also acting on two qubits. CNOT stands for
      <em>Controlled NOT</em>. Qubit one is the <em>control bit</em>, 
      qubit two the <em>target bit</em>. The control bit remains unchanged 
      but will decide how the target bit will be manipulated. If 
      the contol bit is set to {{#latex}}e_1{{/latex}}, the target bit 
      will not be changed (modified by identity {{#latex}}I{{/latex}}). If 
      the contol bit is set to {{#latex}}e_2{{/latex}}, the target bit 
      will be inverted (modified by NOT gate {{#latex}}X{{/latex}}). 
      The CNOT gate {{#latex}}CX{{/latex}} is implemented by
    {{/paragraph}}

    {{#latexeqn_}}
      CX=e_1\otimes e^1\otimes I+e_2\otimes e^2\otimes X\,.
    {{/latexeqn_}}

    {{#paragraph}}
      To analyze how this works, we will inspect both summands. 
      We need to name indices. Let {{#latex}}i{{/latex}} be the 
      covariant index of {{#latex}}CX{{/latex}} that belongs 
      to the control bit and let {{#latex}}p{{/latex}} be the 
      covariant index of {{#latex}}CX{{/latex}} that belongs 
      to the target bit. For the arguments (qubits) we denote the 
      control bit index by {{#latex}}k{{/latex}} and the target bit 
      index by {{#latex}}l{{/latex}}.
      We will distinguish both control bit cases 
      {{#latex}}e_1{{/latex}} and {{#latex}}e_2{{/latex}}, 
      the argument of target bit is denoted by {{#latex}}v{{/latex}}.
    {{/paragraph}}

    {{#latexeqn_}}
      & C_{i,k}(C_{p.l}(e_1\otimes e^1\otimes I\otimes e_1\otimes v))
        = e^1(e_1)e_1\otimes I(v)
        = e_1\otimes v \,, \\
      & C_{i,k}(C_{p.l}(e_2\otimes e^2\otimes X\otimes e_1\otimes v))
        = e^2(e_1)e_2\otimes X(v)
        = 0 \\
    {{/latexeqn_}}

    {{#paragraph}}
      yield
    {{/paragraph}}

    {{#latexeqn_}}
      & CX(e_1\otimes v)=e_1\otimes v
    {{/latexeqn_}}

    {{#paragraph}}
     and
    {{/paragraph}}

    {{#latexeqn_}}
      & C_{i,k}(C_{p.l}(e_1\otimes e^1\otimes I\otimes e_2\otimes v))
        = e^1(e_2)e_1\otimes I(v)
        = 0 \,, \\      
      & C_{i,k}(C_{p.l}(e_2\otimes e^2\otimes X\otimes e_2\otimes v))
        = e^2(e_2)e_2\otimes X(v)
        = e_2\otimes X(v)
    {{/latexeqn_}}

    {{#paragraph}}
      yield
    {{/paragraph}}

    {{#latexeqn_}}
      & CX(e_2\otimes v)=e_2\otimes X(v) \,.
    {{/latexeqn_}}

    {{#paragraph}}
      We want to process the outcome of the previous example. We compute
    {{/paragraph}}

    {{#latexeqn_}}
      CX(e_1\otimes e_1+e_2\otimes e_1)
      &= CX(e_1\otimes e_1)+CX(e_2\otimes e_1) \\
      &= e_1\otimes e_1+e_2\otimes X(e_1) \\
      &= e_1\otimes e_1+e_2\otimes e_2 \,.
    {{/latexeqn_}}

    {{#paragraph}}
      Referring to {{ref "tensors" "exSeparation"}}, we have shown by example, that 
      CNOT maps an unentangled argument to an entangled image. Hence, CNOT can not be 
      expressed by a separable tensor. Otherwise {{ref "tensors" "thmProductMap"}} would 
      ensure that every unentangled argument yields an unentangled image.
    {{/paragraph}}

    {{#paragraph}}
      Most sources on Quantum Computing give the CNOT gate as a 
      {{#latex}}4\times 4{{/latex}}-matrix, e.g. (depending on index order),
    {{/paragraph}}

    {{#latexeqn_}}
      M=
      \begin{bmatrix} 
        1 & 0 & 0 & 0 \\
        0 & 1 & 0 & 0 \\
        0 & 0 & 0 & 1 \\
        0 & 0 & 1 & 0
      \end{bmatrix}\,,
    {{/latexeqn_}}

    {{#paragraph}}
      whereas in our definition the CNOT gate belongs to 
      {{#latex}}(\R^2\otimes\R^2)\otimes(\R^2\otimes\R^2)\,.{{/latex}}
      How does this fit together?
    {{/paragraph}}

    {{#paragraph}}
      Interpreting the tensor as linear map, as we have done, it is an element 
      of {{#latex}}\cL(\R^2\otimes\R^2,\R^2\otimes\R^2){{/latex}}, modifying the 
      (possibly entangled) value of two qubits. In terms of coefficients this 
      linear map is represented by a 4-dimensional array 
      ({{#latex}}2\times 2\times 2\times 2{{/latex}}) and is mapping a 
      {{#latex}}2\times 2{{/latex}}-matrix to a {{#latex}}2\times 2{{/latex}}-matrix.
      But, grouping both covariant indices (inputs of control and target bit) and both 
      contravariant indices (outputs of control and target bit) of the tensor, 
      it stands for a linear map of {{#latex}}\cL(\R^4,\R^4){{/latex}}. We enumerate
      the basis or {{#latex}}\R^4\simeq\R^2\otimes\R^2{{/latex}} 
      in the order (control bit left, target bit right) 
      {{#latex}}
        e_1\otimes e_1,\,e_1\otimes e_2,\,e_2\otimes e_1,\,e_2\otimes e_2
      {{/latex}}. Evaluating the effect of {{#latex}}CX{{/latex}} to these basis 
      elements,
    {{/paragraph}}

    {{#latexeqn_}}
      e_1\otimes e_1\longrightarrow e_1\otimes e_1\,,\\
      e_1\otimes e_2\longrightarrow e_1\otimes e_2\,,\\
      e_2\otimes e_1\longrightarrow e_2\otimes e_2\,,\\
      e_2\otimes e_2\longrightarrow e_2\otimes e_1\,,\\
    {{/latexeqn_}}

    {{#paragraph}}
      we obtain indeed {{#latex}}M{{/latex}} as matrix representation of 
      {{#latex}}CX{{/latex}}.
    {{/paragraph}}

  {{/example}}

  {{#example "exCircuit"}}

    {{#paragraph}}
      A quantum circuit is a sequence of quantum gates. We construct one out of the 
      previous two examples and chain an Hadamard gate and a CNOT gate:
    {{/paragraph}}

    {{#latexeqn_}}
      CX\circ (H\otimes I)\,.
    {{/latexeqn_}}

    {{#paragraph}}
      This represents a simple quantum circuit for obtaining an entangled state out of 
      all-falsy initial configuration.
    {{/paragraph}}

    {{#latexeqn_}}
      (CX\circ (H\otimes I))(e_1\otimes e_1)=e_1\otimes e_1+e_2\otimes e_2\,.
    {{/latexeqn_}}

  {{/example}}

{{/section}}

{{#section "secCheat"}}

  {{#paragraph}}
    We want to reflect and summarize the discoveries of this chapter.
    We had started with the motivation to bring elements of 
    Linear Algebra into a general framework. In good old Computer 
    Science tradition its building blocks are objects and operations 
    on these objects. There is only one type of object, the 
    {{#highlight}}tensor{{/highlight}}, which can act as vector, 
    dual vector or linear map depending on the context. There are 
    two major operations, the {{#highlight}}tensor product{{/highlight}} 
    and the {{#highlight}}contraction{{/highlight}}. The tensor 
    product is used to combine tensors to bigger ones, e.g. a 
    linear map can be built out of the tensor product of a 
    vector and a dual vector. The contraction, however, reduces the 
    size (order) of a tensor by summing along a covariant index 
    and associated contravariant index. It generalizes the 
    matrix-vector-product. Together tensor product and contraction 
    can be used to mimic the application of a linear map to a vector, 
    or to express the composition of linear maps. 
  {{/paragraph}}

  {{#overview "sumLinAlg"}}

    <table>
      <tr>
        <th>Artifact</th>
        <th>Definition</th>
        <th>Place of Residence</th>
      </tr>
      <tr>
        <td>Vector</td>
        <td>{{#latex}}v=\sum\limits_{k=1}^nv^ke_k{{/latex}}</td>
        <td>{{#latex}}V{{/latex}}</td>
      </tr>
      <tr>
        <td>Vector</td>
        <td>{{#latex}}w=\sum\limits_{l=1}^mw^lf_l{{/latex}}</td>
        <td>{{#latex}}W{{/latex}}</td>
      </tr>
      <tr>
        <td>Dual Vector</td>
        <td>{{#latex}}w^\ast=\sum\limits_{l=1}^mw_lf^l{{/latex}}</td>
        <td>{{#latex}}W^\ast{{/latex}}</td>
      </tr>
      <tr>
        <td>Tensor Product</td>
        <td>
          {{#latex}}
            T=\sum\limits_{j=1}^m\sum\limits_{i=1}^nT_i^j\,f_j\otimes e^i
          {{/latex}}
        </td>
        <td>
          {{#latex}}W\otimes V^\ast\simeq V^\ast\otimes W^{\ast\ast}{{/latex}}
        </td>
      </tr>
      <tr>
        <td>Tensor Product</td>
        <td>
          {{#latex}}
            S=\sum\limits_{q=1}^r\sum\limits_{p=1}^mS_p^q\,g_q\otimes f^p
          {{/latex}}
        </td>
        <td>
          {{#latex}}X\otimes W^\ast{{/latex}}
        </td>
      </tr>
      <tr>
        <td>Linear Map</td>
        <td>
          {{#latex}}
            T(v)=C_{i,k}(T\otimes v)
            =\sum\limits_{j=1}^m\sum\limits_{i=1}^nT_i^jv^if_j
          {{/latex}}
        </td>
        <td>
          {{#latex}}\cL(V,W){{/latex}}
        </td>
      </tr>
      <tr>
        <td>Linear Map</td>
        <td>
          {{#latex}}
            S(w)=C_{p,l}(S\otimes w)
            =\sum\limits_{q=1}^r\sum\limits_{p=1}^mS_p^qw^pg_q
          {{/latex}}
        </td>
        <td>
          {{#latex}}\cL(W,X){{/latex}}
        </td>
      </tr>
      <tr>
        <td>Dual Map</td>
        <td>
          {{#latex}}
            T^\ast(w^\ast)=C_{l,j}(T\otimes w^\ast)
            =\sum\limits_{i=1}^n\sum\limits_{j=1}^mT_i^jw_je^i
          {{/latex}}
        </td>
        <td>
          {{#latex}}\cL(W^\ast,V^\ast){{/latex}}
        </td>
      </tr>
      <tr>
        <td>Composed Linear Map</td>
        <td>
          {{#latex}}
            (S\circ T)(v)=C_{p,j}(C_{i,k}(S\otimes T\otimes v))
            =\sum\limits_{q=1}^r\sum\limits_{p=1}^m\sum\limits_{i=1}^n
              S_p^qT_i^pv^ig_q
          {{/latex}}
        </td>
        <td>
          {{#latex}}\cL(V,X){{/latex}}
        </td>
      </tr>
    </table>

  {{/overview}}

  {{#paragraph}}
    As we have pointed out, tensors do not only allow to express concepts 
    of Linear Algebra in a consistent way, they even add multilinearity 
    to those concepts.
  {{/paragraph}}

  {{#overview "sumMulti"}}

    <table>
      <tr>
        <th>Artifact</th>
        <th>Definition</th>
        <th>Place of Residence</th>
      </tr>
      <tr>
        <td>Bilinear Map</td>
        <td>
          {{#latex}}
            T(v_1,v_2)=C_{i_1,k_1}(C_{i_2,k_2}(T\otimes v_1\otimes v_2))
            =\sum\limits_{j=1}^m\sum\limits_{i_1=1}^{n_1}\sum\limits_{i_2=1}^{n_2} 
            T_{i_1i_2}^jv^{i_1}_1v^{i_2}_2f_j
          {{/latex}}
        </td>
        <td>
          {{#latex}}\cL^2(V_1,V_2\,;\,W){{/latex}}
        </td>
      </tr>
      <tr>
        <td>Multilinear Map</td>
        <td>
          {{#latexeqn_}}
            T(v_1,\ldots,v_s) &=
              (C_{i_1,k_1}\circ\ldots\circ C_{i_s,k_s})
              (T\otimes v_1\otimes\ldots\otimes v_s) \\
              &=
                \sum\limits_{j=1}^m
                \sum\limits_{i_1=1}^{n_1}
                \cdots
                \sum\limits_{i_s=1}^{n_s} 
                T_{i_1\ldots i_s}^j
                v^{i_1}_1\cdots v^{i_s}_sf_j
          {{/latexeqn_}}
        </td>
        <td>
          {{#latex}}\cL^s(V_1,\ldots,V_s \, ; \, W){{/latex}}
        </td>
      </tr>
      <tr>
        <td>Tensor Product of Linear Maps</td>
        <td>
          {{#latex}}
            (T\otimes S)(v\otimes x)=T(v)\otimes S(x)
          {{/latex}}
        </td>
        <td>
          {{#latex}}\cL(V\otimes X,W\otimes Y){{/latex}}
        </td>
      </tr>
    </table>

  {{/overview}}

{{/section}}