# Connected Components
## Return colors of the connected components
```cpp
void dfs(vvi& g, int u, vb& visited, vi& colors, int color) {
	visited[u] = true;
    colors[u] = color;
    cout << u << endl;
	for (int i = 0; i < SZ(g[u]); ++i) {
		int v = g[u][i];
		if(not visited[v]) {
			dfs(g, v, visited, colors, color);
		}
	}
}

// color each connected component with a different color
vi dfs_color(vvi &g) {
	int num_cc = 0;
    vb visited(SZ(g), false);
    vi colors(SZ(g), 0);
    for(int v = 0; v < SZ(g); ++v) {
		if(!visited[v]) {
            ++num_cc;
			dfs(g, v, visited, colors, num_cc);
		}
	}
    return colors;
}
```
## Return the connected components
```cpp
void dfs(vvi& g, int u, vb& visited, vi& cc, int color) {
	visited[u] = true;
    cc.pb(u);
	for (int i = 0; i < SZ(g[u]); ++i) {
		int v = g[u][i];
		if(not visited[v]) {
			dfs(g, v, visited, cc, color);
		}
	}
}

vvi dfs_color(vvi &g) {
	int num_cc = 0;
    vb visited(SZ(g), false);
    vvi cc;
    for(int v = 0; v < SZ(g); ++v) {
		if(!visited[v]) {
            cc.pb({});
			dfs(g, v, visited, cc[num_cc], num_cc);
            ++num_cc;
		}
	}
    return cc;
}
```