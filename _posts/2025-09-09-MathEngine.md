---
title: MathEngine
date: 2025-09-09
categories: ["Projects"]
tags: ["Featured Projects"]
---

[MathEngine](https://github.com/User0332/MathEngine) is a set of .NET class libraries that allow symbolic computation in C#, similar to Wolfram Mathematica.

MathEngine's goal is to keep everything **pure**. *No approximations*, *no numeric solutions*, and everthing is solved *strictly analytically* to provide mathematically solid results. The key motivation for creating such a system is:

*If a human can solve it with a set of strategies, a machine can too.*

In fact, it can apply those strategies thousands of times faster!

MathEngine's docs are available on [Read the Docs](https://mathengine.readthedocs.io), and the library itself is available on [NuGet](https://www.nuget.org/packages?q=MathEngine+User0332&includeComputedFrameworks=true&packagetype=dependency&prerel=true&sortby=relevance).


# Algebra Examples

MathEngine's abilities as an emerging computer algebra system already include effective handling of polynomials, as seen in the examples below:

## Analyzing a Polynomial Expression

```cs
using MathEngine.Algebra;
using MathEngine.Algebra.Expressions.Polynomial;

var x = Variable.X;

var expr = PolynomialExpression.From(
	(x^2) + 2*x + 1 + (-4*x)
);

NormalizedPolynomialExpression normalized = expr.Normalize();

foreach (var term in normalized.NormalizedTerms)
{
	Console.WriteLine($"Degree of Term: {NormalizedPolynomialExpression.DegreeOfNormalizedTerm(term)}");
	Console.WriteLine($"Coefficient of Term: {NormalizedPolynomialExpression.CoefficientOfNormalizedTerm(term)}");
}
```

## Solving a Polynomial Equation


```cs
using MathEngine.Algebra;
using MathEngine.Algebra.Solver.Polynomial;

var x = Variable.X;

var eq = ((x^2)+5*x+6).SetEqualTo(2*x+8).ToPolynomial();

var solns = eq.Solve(strategy: PolynomialSolvingStrategy.UseFormula).Select(soln => soln.Simplify()).Distinct();

foreach (var soln in solns)
{
	Console.WriteLine(soln.LaTeX());
}
```

## Simplifying an Expression

```cs
using MathEngine.Algebra;
using MathEngine.Algebra.Expressions.Simplification;

var x = Variable.X;

var expr = (x^2) + 2*x + 1 + (-4*x)/2 - 3;

Console.WriteLine(expr);
Console.WriteLine(expr.Simplify(SimplificationStrategy.Default)); /* SimplificationStrategy.Default is the default/implicit parameter value, so it does not need to be passed. If you choose not to use SimplificationStrategy, you do not need to use the namespace MathEngine.Algebra.Expressions.Simplification either */
```

## Substituting in for a Variable

```cs
using MathEngine.Algebra;
using MathEngine.Algebra.Expressions;

Variable h = new('h');

Expression expr = (h^4)+2*h+3;

Console.WriteLine(expr.Substitute(h, Expression.OneHalf).Simplify());
Console.WriteLine(expr.Substitute(Variable.T, Expression.OneHalf).Simplify()); /* does nothing because the variable 't' is not in the expression */
```