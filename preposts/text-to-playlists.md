# Greedy algorithm

Take the text in the form of a list of words $W= (w_1, w_2, \ldots, w_n)$, and initialize a list $P$ that will save the songs for the playlist.

1. Set $k = j = 1$ and $s = w_1$.
2. Look for $s$.
3. Case $k + j < n$. If there is a match for $s$, add the song  to $P$, update  $j = 0$, $k = k+1$ and set $s = w_k$. If there isn't a match, update $j = j+1$ and set $s = s + w_{k+j}$. In both cases go to 2.
4. Case $k+j = n$. If there is a match, we are done, return the playlist $P$. If there is not, return a fail.



This algorithm cannot be implemented in Spotify, or not as easily, because the search tool does not have an option for retrieving exact matches.