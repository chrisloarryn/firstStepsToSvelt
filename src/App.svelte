<script>
  let products = [
    {
      id: 1,
      name: 'HP Pavilion Notebook',
      description: 'HP Laptop',
      category: 'laptop',
    },
    {
      id: 2,
      name: 'Mouse Razer',
      description: 'Razer Gaming Mouse',
      category: 'Peripherals',
    },
  ]
  let product = {
    id: '',
    name: '',
    description: '',
    category: '',
    imageURL: '',
  }

  const onSubmitHandler = (e) => {
    e.preventDefault()
    console.log(product)
  }
</script>

<style>

</style>

<main>
  <div class="container">
    <div class="row">
      <div class="col-md-6">
        {#each products as product}
          <div class="card mt-2">
            <div class="row">
              <div class="col-md-4">
                <img src="" alt="" />
              </div>
              <div class="col-md-8">
                <div class="card-body">
                  <h5>
                    <strong>{product.name}</strong>
                    <span>
                      <small>{product.category}</small>
                    </span>
                  </h5>
                  <p class="card-text">{product.description}</p>
                  <button class="btn btn-danger">Delete</button>
                  <button class="btn btn-secondary">Edit</button>
                </div>
              </div>
            </div>
          </div>
        {/each}
      </div>
      <div class="col-md-6">
        <div class="card-body">
          <form on:submit={onSubmitHandler}>
            <div class="form-group">
              <input
                bind:value={product.name}
                type="text"
                placeholder="Product Name"
                id="productName"
                class="form-control" />
            </div>
            <div class="form-group">
              <textarea
                bind:value={product.description}
                id="productDescription"
                placeholder="Product Description"
                rows="3"
                class="form-control" />
            </div>
            <div class="form-group">
              <input
                bind:value={product.imageURL}
                type="url"
                placeholder="https://github.com/chrisloarryn"
                id="productImageUrl"
                class="form-control" />
            </div>
            <div class="form-group">
              <select
                bind:value={product.category}
                id="cat"
                name="cat"
                class="form-control">
                <option value="laptops" selected>Laptops</option>
                <option value="peripherals">Peripherals</option>
                <option value="servers">Servers</option>
              </select>
            </div>
            <button class="btn btn-secondary">Save Product</button>
          </form>
        </div>
      </div>
    </div>
  </div>
</main>
