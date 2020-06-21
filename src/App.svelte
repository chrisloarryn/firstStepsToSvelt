<script>
  import { v4 } from "uuid";
  import Noty from "noty";
  import "noty/lib/noty.css";
  import "noty/lib/themes/sunset.css";

  let products = [
    {
      id: "cb36f2be-2093-4121-8da5-70c79bcbd104",
      name: "HP Pavilion Notebook",
      description: "HP Laptop",
      category: "Laptop",
      imageURL: ""
    },
    {
      id: "1d96f130-f9e8-4d72-91cc-13c8c467d862",
      name: "Mouse Razer",
      description: "Razer Gaming Mouse",
      category: "Peripherals",
      imageURL: ""
    },
    {
      category: "Peripherals",
      description: "keyboard",
      id: "9bd54ac4-8bb3-4470-ac49-7303a889f36c",
      imageURL:
        "https://images-na.ssl-images-amazon.com/images/I/71kyDym6QSL._AC_SL1500_.jpg",
      name: "MSI"
    }
  ];

  let editStatus = { status: false, id: "" };

  let product = {
    id: "",
    name: "",
    description: "",
    category: "",
    imageURL: ""
  };

  const cleanProduct = () => {
    product = {
      id: "",
      name: "",
      description: "",
      category: "",
      imageURL: ""
    };
  };

  const cleanStatus = () => {
    editStatus.status = false;
    editStatus.id = "";
  };

  const addProduct = () => {
    const prodId = v4();
    if (!product.category || !product.name || !product.description)
      return new Noty({
        theme: "sunset",
        type: "warning",
        timeout: 3000,
        text: "Product fields cannot be empty!"
      }).show();
    // console.log(prodId);
    const newProduct = {
      ...product,
      id: prodId
    };
    products = products.concat(newProduct);
    cleanProduct();
    // console.log(products);
  };

  const updateProduct = () => {
    // let updatedProduct = {...product} but instead I assign each one
    let updatedProduct = {
      name: product.name,
      description: product.description,
      id: product.id,
      imageURL: product.imageURL,
      category: product.category
    };
    const productIndex = products.findIndex(p => p.id == product.id);
    products[productIndex] = updatedProduct;
    new Noty({
      theme: "sunset",
      type: "success",
      timeout: 3000,
      text: "Product updated successfully!"
    }).show();
    cleanProduct();
    cleanStatus();
    // console.log(productIndex);
  };

  const onSubmitHandler = e => {
    if (!editStatus.status) addProduct();
    else updateProduct();
  };

  const deleteProduct = productId => {
    products = products.filter(p => p.id != productId);
    new Noty({
        theme: "sunset",
        type: "success",
        timeout: 3000,
        text: "Product deleted Successfully!"
      }).show();
    if (productId === editStatus.id) {
      cleanProduct();
    }
    cleanStatus();
  };

  const editProduct = editedProduct => {
    // assign product to editing product form
    product = editedProduct;
    // save variables to check status
    editStatus.status = !editStatus.status;
    editStatus.id = editedProduct.id;

    if (!editStatus.status && editStatus.id) {
      cleanProduct();
      editStatus.id = "";
    }
    // console.log(product, editStatus);
  };
</script>

<style>

</style>

<main>
  <nav class="navbar navbar-expand-lg navbar-light bg-light">
  <!-- <a class="navbar-brand" href="#">Navbar</a> -->
  <img src="images/bootstrap-solid.svg" width="30" height="30" class="d-inline-block align-top" alt="">
  <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon"></span>
  </button>

  <div class="collapse navbar-collapse" id="navbarSupportedContent">
    <ul class="navbar-nav mr-auto">
      <li class="nav-item active">
        <a class="nav-link" href="#">Home <span class="sr-only">(current)</span></a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="#">Link</a>
      </li>
      <li class="nav-item dropdown">
        <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
          Dropdown
        </a>
        <div class="dropdown-menu" aria-labelledby="navbarDropdown">
          <a class="dropdown-item" href="#">Action</a>
          <a class="dropdown-item" href="#">Another action</a>
          <div class="dropdown-divider"></div>
          <a class="dropdown-item" href="#">Something else here</a>
        </div>
      </li>
      <li class="nav-item">
        <a class="nav-link disabled" href="#" tabindex="-1" aria-disabled="true">Disabled</a>
      </li>
    </ul>
    <form class="form-inline my-2 my-lg-0">
      <input class="form-control mr-sm-2" type="search" placeholder="Search" aria-label="Search">
      <button class="btn btn-outline-success my-2 my-sm-0" type="submit">Search</button>
    </form>
  </div>
</nav>
  <div class="container p-4">
    
    <div class="row">
      <div class="col-md-6">
        {#each products as product}
          <div class="card mt-2">
            <div class="row">
              <div class="col-md-4">
                {#if !product.imageURL}
                  <img
                    class="img-fluid p-2"
                    src="./images/not-found.png"
                    alt="" />
                {:else}
                  <img class="img-fluid p-2" src={product.imageURL} alt="" />
                {/if}
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
                  <button
                    class="btn btn-danger"
                    on:click={deleteProduct(product.id)}>
                    Delete
                  </button>
                  <button
                    class="btn btn-secondary"
                    on:click={editProduct(product)}>
                    Edit
                  </button>
                </div>
              </div>
            </div>
          </div>
        {/each}
      </div>
      <div class="col-md-6">
        <div class="card-body">
          <form on:submit|preventDefault={onSubmitHandler}>
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
                <option value="Laptops" selected>Laptops</option>
                <option value="Peripherals">Peripherals</option>
                <option value="Servers">Servers</option>
              </select>
            </div>
            <button class="btn btn-secondary">
              {#if !editStatus.status}Save Product{:else}Update Product{/if}
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</main>
