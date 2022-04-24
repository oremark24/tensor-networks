---
layout: chapter.hbs
tags: chapter
---

{{#section "secDiagrams"}}

  {{#paragraph}}
    In the previous chapter we have introduced tensors and operations on 
    tensors. Tensor products and contractions offer ways to compose 
    existing tensors to new ones. Consider for example the tensor of 
    order-{{#latex}}(2,1){{/latex}} tensor
  {{/paragraph}}

  {{#latexeqn "eqnNetwork"}}
    N=
    \sum\limits_{i=1}^n  
    \sum\limits_{k=1}^r
    \sum\limits_{l=1}^s
    \sum\limits_{j=1}^m
    T^i_jS^{jl}_k\,e_i\otimes f^k\otimes g_l
    \,.
  {{/latexeqn}}

  {{#paragraph}}
    It was composed from an order-{{#latex}}(1,1){{/latex}} tensor
    {{#latex}}T{{/latex}} and an order-{{#latex}}(2,1){{/latex}} tensor
    {{#latex}}S{{/latex}}. Both have been combined by a 
    tensor product (yielding 5 tensor indices) and then zipped by a contraction 
    (yielding 3 remaining tensor indices and the summation index 
    {{#latex}}j{{/latex}}).
  {{/paragraph}}

  {{#paragraph}}
    In literature various opinions (definitions) are circulating on what 
    <em>tensor networks</em> are. Some call already a decomposition of a 
    tensor by means of tensor products and contractions a tensor networks.
    For example the <em>Matrix Product State</em>, a decomposition of a 
    tensor into a sum of matrix products is often referred as a simplistic 
    instance of a tensor network. Others connect the term tensor network with 
    the graphical language (notation) we will introduce shortly. Indeed,
    <em>tensor network diagrams</em> join shapes representing tensors with 
    wires. Hence, such diagrams display networks of tensors wired together.
    Defining the name is not the most important thing, so we will give a 
    shaky definition that follows the notion of decomposition into tensors, 
    but use tensor networks from now on always in conjunction with 
    illustrations exploiting the graphical language.
  {{/paragraph}}

  {{#definition "defNetwork"}}

    {{#paragraph}}
      A {{#highlight}}tensor network{{/highlight}} is a tensor, composed from 
      other tensors using tensor products and contractions.
    {{/paragraph}}

  {{/definition}}

  {{#paragraph}}
    Having clarified this, let us straight away jump into the graphical notation 
    for tensor networks. It was introduced by Roger Penrose in 
    {{cite "Pen71"}}. Jacob Biamonte has refined the way certain shapes underpin 
    the structure of tensors {{cite "Bia19"}}, we will follow mostly his suggestions.
  {{/paragraph}}

  {{#overview "illuDiagrams"}}

    {{#paragraph}}
      Each tensor is represented by a geometric shape. Indices are represented by 
      legs connected to the shape. We will distinguish covariant and contravariant 
      indices by consideration of the leg direction. Covariant legs point to the 
      left or up, contravariant legs point to the right or down. The tensor
    {{/paragraph}}

    {{#latexeqn_}}
      T=\sum\limits_{i=1}^n\sum\limits_{j=1}^mT^i_j\,e_i\otimes h^j
    {{/latexeqn_}}

    {{#paragraph}}
      can be drawn with left-right orientation as follows.
    {{/paragraph}}

    {{imageHeight "tensor_networks/intro-t.svg" "52em"}}

    {{#paragraph}}
      Orienting the legs up-down and describe tensor
    {{/paragraph}}

    {{#latexeqn_}}
      S=\sum\limits_{k=1}^r\sum\limits_{p=1}^m\sum\limits_{l=1}^s
        S^{pl}_k\,f^k\otimes h_p\otimes g_l
    {{/latexeqn_}}

    {{#paragraph}}
      as seen here.
    {{/paragraph}}
    
    {{imageHeight "tensor_networks/intro-s.svg" "102em"}}

    {{#paragraph}}
      Also having mixed orientations is possible.
    {{/paragraph}}

    {{imageHeight "tensor_networks/intro-s2.svg" "75em"}}

    {{#paragraph}}
      The tensor product is displayed by drawing the factor 
      tensor next to each other. We draw
    {{/paragraph}}

    {{#latexeqn_}}
      T\otimes S=
        \sum\limits_{i=1}^n
        \sum\limits_{j=1}^m
        \sum\limits_{k=1}^r
        \sum\limits_{p=1}^m
        \sum\limits_{l=1}^s
        T^i_jS^{pl}_k\,
        e_i\otimes h^j\otimes f^k\otimes h_p\otimes g_l
    {{/latexeqn_}}

    {{#paragraph}}
      as follows.
    {{/paragraph}}

    {{imageHeight "tensor_networks/intro-t-prod-s.svg" "110em"}}

    {{#paragraph}}
      The contraction of two indices will be represented by connecting 
      the respective legs. We will call this a wire. The tensor of equation 
      {{ref "tensor_networks" "eqnNetwork"}} is obtained by
    {{/paragraph}}
    
    {{#latexeqn_}}N=C_{j,p}(T\otimes S){{/latexeqn_}} 
    
    {{#paragraph}}
      and can be displayed as shown in this picture.
    {{/paragraph}}

    {{imageHeight "tensor_networks/intro-t-prod-s-contracted.svg" "110em"}}

    {{#paragraph}}
      The naming of indices is an implementation detail that might be omitted 
      by not labeling certain (or all) legs. In general objects might not be 
      named if not in focus. Furthermore we will be free to 
      use symbols, containers, etc. if it supports understanding.
    {{/paragraph}}

    {{imageHeight "tensor_networks/intro-equation.svg" "130em"}}  

  {{/overview}}

  {{#paragraph}}
    So far the general alphabet of graphical tensor language is already defined.
    We will now go over some specific words.
  {{/paragraph}}

  {{#overview "illuVectors"}}

    {{#paragraph}}
      Vectors are represented by triangular shapes, e.g.
    {{/paragraph}}

    {{#latexeqn_}}
      v=\sum\limits_{i=1}^nv^ie_i\in V \,.
    {{/latexeqn_}}

    {{imageHeight "tensor_networks/vectors-basic.svg" "90em"}}   

    {{#paragraph}}
      We will apply this shape also to elements from tensor product spaces
      that have only covariant indices, e.g. 
    {{/paragraph}}

    {{#latexeqn_}}
      &T=\sum\limits_{i=1}^n\sum\limits_{j=1}^m
        T^{ij}\,e_i\otimes f_j
        \in V\otimes W \,, \\
      &S=\sum\limits_{i=1}^n\sum\limits_{j=1}^m\sum\limits_{k=1}^r
        S^{ijk}\,e_i\otimes f_j\otimes g_k
        \in V\otimes W\otimes X \,.
    {{/latexeqn_}}

    {{imageHeight "tensor_networks/vectors-product.svg" "90em"}}     

  {{/overview}}

  {{#overview "illuDuals"}}

    {{#paragraph}}
      Contrarily, the shape is flipped when standing for an element of 
      a dual space or tensor product of dual spaces, e.g.
    {{/paragraph}}

    {{#latexeqn_}}
      &v=\sum\limits_{i=1}^nv_ie^i\in V^\ast \,, \\ 
      &T=\sum\limits_{i=1}^n\sum\limits_{j=1}^m
        T_{ij}\,e^i\otimes f^j
        \in V^\ast\otimes W^\ast \,.
    {{/latexeqn_}}

    {{imageHeight "tensor_networks/vectors-dual.svg" "90em"}}  

  {{/overview}}

  {{#overview "illuGeneral"}}

    {{#paragraph}}
      All tensors combining vector parts and dual parts will usually 
      be displayed as a square - with exception of specific tensors 
      that shall be visually distinguishable. We have already seen 
      examples in {{ref "tensor_networks" "illuDiagrams"}}, let us 
      display tensor 
    {{/paragraph}}

    {{#latexeqn_}}
      N= 
        \sum\limits_{i=1}^n\sum\limits_{k=1}^r\sum\limits_{l=1}^s 
        N^{il}_k\,e_i\otimes f^k\otimes g_l
    {{/latexeqn_}}

    {{#paragraph}}
      from {{ref "tensor_networks" "eqnNetwork"}} again.
    {{/paragraph}}

    {{imageHeight "tensor_networks/general-n.svg" "60em"}}

    {{#paragraph}}
      A first species that will own a separate shape to reveal
      its specifics are tensors with diagonal coefficients matrix,
      e.g.
    {{/paragraph}}

    {{#latexeqn_}}
      d=\sum\limits_{i=1}^nd^i_i\,e_i\otimes e^i\in V\otimes V^\ast\,.
    {{/latexeqn_}}

    {{#paragraph}}
      These diagonal tensors are, so to speak, a hybrid of a vector and 
      a tensor having a covariant as well as a contravariant index. This 
      is reflected by combining the shapes of a vector and a dual vector 
      into a diamond shape.
    {{/paragraph}}

    {{imageHeight "tensor_networks/general-diagonal.svg" "75em"}}

  {{/overview}}

  {{#paragraph}}
    Now we are ready to combine tensors by connecting legs (indices) with 
    wires (contractions). A fundamental ingredient of Linear Algebra 
    are linear maps. In the previous chapter we have seen, how they can 
    be formulated in terms of tensors. We will translate the formulas 
    obtained there into the graphical tensor language. The outcome 
    will match our intuition.
  {{/paragraph}}

  {{#overview "illuLinMaps"}}

    {{#paragraph}}
      We recall, that we can write the application of a linear map 
      {{#latex}}T{{/latex}} to a vector {{#latex}}v{{/latex}} as 
      the tensor product of both regarded as tensors:
    {{/paragraph}}

    {{#latexeqn_}}
      T(v)=C_{i,k}(T\otimes v)\,.
    {{/latexeqn_}}

    {{#paragraph}}
      Hence, this can be drawn as follows.
    {{/paragraph}}

    {{imageHeight "tensor_networks/lin-maps-Tv.svg" "90em"}}

    {{#paragraph}}
      Similarly, we can visualize the application of a dual map 
    {{/paragraph}}

    {{#latexeqn_}}
      T^\ast(v^\ast)=C_{k,j}(T\otimes v^\ast)
    {{/latexeqn_}}

    {{#paragraph}}
      as:
    {{/paragraph}}

    {{imageHeight "tensor_networks/lin-maps-vT.svg" "90em"}}

    {{#paragraph}}
      Actually, the same tensor can be interpreted in both ways.
      If we connect the covariant index with a vector, then it 
      is acting as linear map. The remaining open index of the 
      construct is a contravariant index, symbolizing the 
      image vector. If we connect the contravariant index of the 
      tensor with a dual vector, then we have visualized 
      the application of a dual linear map. The resulting image
      possesses a covariant index, thus being the dual image 
      vector.
    {{/paragraph}}

    {{#paragraph}}
      Chaining linear maps is straight forward as well. This is 
      achieved by contracting the tensor product of the tensors 
      representing the maps.
    {{/paragraph}}

    {{#latexeqn_}}
      (S\circ T)(v)=C_{p,j}(C_{i,k}(S\otimes T\otimes v))
    {{/latexeqn_}}

    {{#paragraph}}
      Hence, the picture shows what intution expects, the 
      two tensors symbolizing the maps have to be wired together.
    {{/paragraph}}

    {{imageHeight "tensor_networks/lin-maps-STv.svg" "90em"}}

    {{#paragraph}}
      Hence, the picture shows what intution expects, the 
      two tensors symbolizing the maps have to be wired together.
    {{/paragraph}}

    {{#paragraph}}
      A bilinear map
    {{/paragraph}}

    {{#latexeqn_}}
      T(v_1,v_2)=C_{i_1,k_1}(C_{i_2,k_2}(T\otimes v_1\otimes v_2))
    {{/latexeqn_}}

    {{#paragraph}}
      extends this picture simply by adding and contracting the second argument
      to the map tensor.
    {{/paragraph}}
    
    {{imageHeight "tensor_networks/lin-maps-Tvw.svg" "120em"}}

    {{#paragraph}}
      Multilinear maps would extend this picture even further by adding as many 
      argument vectors as needed.
    {{/paragraph}}

    {{#paragraph}}
      In case that the map tensor can be decomposed into a tensor product, 
      enabling equation
    {{/paragraph}}

    {{#latexeqn_}}
      (T_1\otimes T_2)(v_1\otimes v_2)=T_1(v_1)\otimes T_2(v_2)\,,
    {{/latexeqn_}}

    {{#paragraph}}
      the decomposition is shown as two maps next to each other. Note, that 
      this complies with the figure of a tensor product - both 
      tensors will be drawn independently without any interaction (as long as no 
      contraction is involved).
    {{/paragraph}}

    {{imageHeight "tensor_networks/lin-maps-TvSw.svg" "120em"}}

  {{/overview}}

  {{#overview "illuScalar"}}

    {{#paragraph}}
      So far we have not considered the possibility of a scalar
      (element of the underlying field). 
      Since a scalar value can be treated as tensor without 
      indices, it is exactly drawn like this, as shape without 
      legs.
    {{/paragraph}}

    {{imageHeight "tensor_networks/scalar.svg" "50em"}}

    {{#paragraph}}
      Having used a circle is arbitrary - actually it does not 
      matter, because we will very rarely draw isoloated 
      scalar tensors. More common is the case, that a complex 
      tensor network is fully contracted to a scalar. We give a 
      few examples (with limited complexity though) here.
      Referring to {{ref "tensors" "exMatrixContraction"}}, the 
      trace of a matrix can be expressed as fully contracted 
      tensor:
    {{/paragraph}}

    {{#latexeqn_}}
        \tr(T)=C_{i,j}(T) \,.
    {{/latexeqn_}}

    {{imageHeight "tensor_networks/scalar-trace.svg" "72em"}}

    {{#paragraph}}
      Using the way of visualizing composed linear maps, 
      the <em>trace</em> of a matrix product (composed linear maps)
      would be:
    {{/paragraph}}

    {{imageHeight "tensor_networks/scalar-trace2.svg" "76em"}}

    {{#paragraph}}
      Another operation from Linear Algebra that results in a 
      scalar value is the <em>inner product</em>. We consider the 
      case of the inner product space being {{#latex}}\C^n{{/latex}}
      equipped with the standard basis (which is orthonormal)
      {{#latex}}\{e_1,\ldots,e_n\}{{/latex}}. We can calculate the 
      inner product of 
      {{#latex}}v=\sum\limits_{i=1}^nv^ie_i{{/latex}} and 
      {{#latex}}w=\sum\limits_{j=1}^nw^je_j{{/latex}} by their coefficients:
    {{/paragraph}}

    {{#latexeqn_}}
      \ip{v}{w} 
      &= \IP{\sum\limits_{i=1}^nv^ie_i}{\sum\limits_{j=1}^nw^je_j} \\
      &= \sum\limits_{i=1}^n\sum\limits_{j=1}^n
        \bar{v}^iw^j\underbrace{\ip{e_i}{e_j}}_{=\delta_{ij}} \\
      &= \sum\limits_{i=1}^n\bar{v}^iw^i\,.
    {{/latexeqn_}}

    {{#paragraph}}
      On the other hand, let 
      {{#latex}}\bar{v}=\sum\limits_{i=1}^n\bar{v}_ie^i{{/latex}} 
      be a dual vector, 
      that has the conjugate complex coefficients of {{#latex}}v{{/latex}}
      (i.e. {{#latex}}\bar{v}^i=\bar{v}_i{{/latex}})
      in the dual standard basis.      
      We calculate
    {{/paragraph}}

    {{#latexeqn_}}
      C_{i,j}(\bar{v}\otimes w)
      &= C_{i,j}\Bigg(\sum\limits_{i=1}^n\sum\limits_{j=1}^n
        \bar{v}_iw^j\,e^i\otimes e_j\Bigg) \\
      &= \sum\limits_{i=1}^n\sum\limits_{j=1}^n
        \bar{v}_iw^j\underbrace{e^i(e_j)}_{=\delta^i_j} \\
      &= \sum\limits_{i=1}^n\underbrace{\bar{v}_i}_{=\bar{v}^i}w^i \\
      &= \ip{v}{w} \,.
    {{/latexeqn_}}

    {{#paragraph}}
      Thus, the inner product of {{#latex}}v{{/latex}} and {{#latex}}w{{/latex}} 
      can be expressed with a tensor product using {{#latex}}\bar{v}{{/latex}} instead 
      of {{#latex}}v{{/latex}}. The tensor network diagram can be drawn accordingly.
    {{/paragraph}}

    {{imageHeight "tensor_networks/scalar-ip.svg" "90em"}}

  {{/overview}}

  {{#paragraph}}
    With this we have introduced the basic building blocks of tensor network
    diagrams. As we see, they provide a bird's eye perspective 
    on tensor networks. Certain implementation details are abstracted away 
    from the viewer, such as index labels or index order. For example, the 
    tensor behind 
  {{/paragraph}}

  {{imageHeight "tensor_networks/general-t.svg" "60em"}}

  {{#paragraph}}
    could be     
  {{/paragraph}}

  {{#latexeqn_}}
    T=\sum\limits_{i=1}^n\sum\limits_{j=1}^m\sum\limits_{k=1}^r 
      T^{ij}_k\,e_i\otimes f_j\otimes g^k \in V\otimes W\otimes X^\ast
      \,,
  {{/latexeqn_}}

  {{#paragraph}}
    or with other index names
  {{/paragraph}}

  {{#latexeqn_}}
    T=\sum\limits_{p=1}^n\sum\limits_{q=1}^m\sum\limits_{t=1}^r 
      T^{pq}_t\,e_p\otimes f_q\otimes g^t \in V\otimes W\otimes X^\ast
      \,,
  {{/latexeqn_}}

  {{#paragraph}}
    or changing the index order 
  {{/paragraph}}

  {{#latexeqn_}}
    T=\sum\limits_{k=1}^r \sum\limits_{j=1}^m\sum\limits_{i=1}^n
      T^{ji}_k\,g^k\otimes f_j\otimes e_i \in X^\ast\otimes W\otimes V
      \,,
  {{/latexeqn_}}

  {{#paragraph}}
    or an object in another space 
  {{/paragraph}}

  {{#latexeqn_}}
    T=\sum\limits_{i=1}^n\sum\limits_{j=1}^n\sum\limits_{k=1}^n
      T^{ij}_k\,e_i\otimes e_j\otimes e^k \in \R^n\otimes\R^n\otimes\R^n
      \,.
  {{/latexeqn_}}

  {{#paragraph}}
    However, we want to emphasize that certain properties are strict and do 
    not allow for ambiguity. The following tensor network diagrams 
    refer to different object because the amounts of covariant and contravariant 
    (the tensor's order) do matter.
  {{/paragraph}}

  {{imageHeight "tensor_networks/general-neq.svg" "60em"}}

  {{#paragraph}}
    But we need to be careful, indices might not 
    be interchangeable when tensor's internal mode of operation
    is known and not symmetric. Consider e.g.
    {{#latex}}T=e^1\otimes e^2\in\R^2\otimes\R^2{{/latex}}
    with {{#latex}}\{e^1,e^2\}{{/latex}} being dual standard 
    basis. Then the diagrams
  {{/paragraph}}

  {{imageHeight "tensor_networks/general-neq2.svg" "160em"}}

  {{#paragraph}}
    are not equal, the left one contracts to value {{#latex}}1{{/latex}}, 
    the right one contracts to value {{#latex}}0{{/latex}}.
  {{/paragraph}}

{{/section}}

{{#section "secKronecker"}}

  {{#paragraph}}
    {{#latex}}\delta^i_j{{/latex}} ...
  {{/paragraph}}

{{/section}}

{{#section "secLevi"}}

  {{#paragraph}}
    {{#latex}}\varepsilon_{ijk}{{/latex}} ...
  {{/paragraph}}

{{/section}}



    