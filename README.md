# laravel-auto-create-db
It helps to auto create dabase table coulmns depending on client request. It is best for advance development. 

## Warnning 
Don't use on production mode, it may be the reason of garbase values.

### Controller Code 

<code>
  function functionName(Request $req)
    {
        $data = [];
        foreach ($req->post() as $field => $value) {
            if ($field != "_token") {
                if (Schema::hasColumn('products', $field)) {
                    $data[$field] = $value;
                } else {
                    Schema::table('products', function ($table) use ($field) {
                        $table->string($field)->nullable();
                    });
                    $data[$field] = $value;
                }
            }
        }
        DB::table('products')->insert($data);

        $id = DB::getPdo()->lastInsertId();
        return redirect("/redirect-page/$id")->with('success', 'Added successfully!');
    }
</code>
