 // Método 1
    public function index()
    {
        $albums = Album::all()->paginate(10);
        return view('albums.index', compact('albums'));
    }

El error en el método 1 se puede encontrar en "$albums = Album::all()->paginate(10);"
No se pueden paginar 10 elementos y a la vez llamar a todos os objetos álbum,hay que indicar en que tipo de órden quieres que se muestren por cada paginación,   esas 2 funciones no pueden ir seguidas de modo que dará error.
Versión correcta: 
 public function index()
    {
        $albums = Album::orderByDesc('created_at')->paginate(10);
        return view('albums.index', compact('albums'));
    }







    // Método 2
    public function update(Request $request, Album $album)
    {
        if (auth()->id() !== $album->user_id || auth()->user()->isAdmin()) {
            abort(403);
        }

        $album->update($request->validated());

        return redirect()->route('albums.show', $album)
            ->with('success', 'Álbum actualizado.');
    }








    // Método 3
    public function show(Album $album)
    {
        $reviews = $album->reviews()->orderBy('created_at', 'desc')->get();
        return view('albums.show', compact('album'));
    }

    Calculas la variable reviews pero nunca la mandas al show, tan solo mandas el álbum que quieres que se vea en el show.
    



