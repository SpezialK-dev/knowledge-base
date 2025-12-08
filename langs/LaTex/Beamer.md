

# removing navigation buttons

https://williamspaniel.com/2014/09/02/just-say-no-to-beamer-navigaton-buttons/


```Latex
\setbeamertemplate{navigation symbols}{}
```

needs to be added after your begin Document


# lstlisting

```
! Paragraph ended before \lst@next was complete.
```

If you want to use a lstlistings enviroment but you get the above showen error you need to do the following thing you need to add a fragile tag to your frame as showen below 


```LaTeX
\begin{frame}[fragile]
	% other LaTeX stuff
 \begin{lstlisting}
	some code 
    \end{lstlisting}
% More LaTeX Code potentially
\end{frame}

```