<template>
	<div class="container">
		<div class="row">
			<div class="col-md-3">
				<div class="sidebar">
					<h4>Categories</h4>
					<button @click="toggleSortOrder">Toggle Sort Order</button>
					<ul>
						<li v-for="category in sortedCategories" :key="category.id">
							<router-link :to="`/products/${category.id}`">{{ category.name }}</router-link>
						</li>
					</ul>
				</div>
			</div>
			<div class="col-md-9">
				<div class="search-bar">
					<input type="text" v-model="searchQuery" placeholder="Search products">
				</div>
				<div class="products">
					<div class="row">
						<div class="col-md-4" v-for="product in filteredProducts" :key="product.id">
							<div class="card">
								<img :src="product.image" class="card-img-top" alt="Product Image">
								<div class="card-body">
									<h5 class="card-title">{{ product.name }}</h5>
									<p class="card-text">{{ product.description }}</p>
									<p class="card-price">Price: ${{ product.price }}</p>
									<div class="card-actions">
										<input type="number" v-model="product.quantity" min="1">
										<button class="btn btn-primary" @click="addToCart(product)">Add to Cart</button>
									</div>
								</div>
							</div>
						</div>
					</div>
				</div>
				<div class="pagination">
					<ul class="pagination justify-content-center">
						<li class="page-item" :class="{ 'active': currentPage === page }" v-for="page in totalPages" :key="page">
							<a class="page-link" href="#" @click="changePage(page)">{{ page }}</a>
						</li>
					</ul>
				</div>
			</div>
		</div>
	</div>
</template>
  
<script>
export default {
	name:"ProductComponent",
	data() {
		return {
			categories: [],
			products: [],
			searchQuery: '',
			sortOrderAsc: true,
			currentPage: 1,
			itemsPerPage: 9,
			cart: [],
		};
	},
	computed: {
		sortedCategories() {
			// return []
			return this.categories.sort((a, b) => {
				if (this.sortOrderAsc) {
					return a.name.localeCompare(b.name);
				} else {
					return b.name.localeCompare(a.name);
				}
			});
		},
		filteredProducts() {
			const filtered = this.products.filter(product => {
				return product.name.toLowerCase().includes(this.searchQuery.toLowerCase());
			});
			const startIndex = (this.currentPage - 1) * this.itemsPerPage;
			return filtered.slice(startIndex, startIndex + this.itemsPerPage);
		},
		totalPages() {
			return Math.ceil(this.filteredProducts.length / this.itemsPerPage);
		},
	},
	methods: {
		toggleSortOrder() {
			this.sortOrderAsc = !this.sortOrderAsc;
		},
		changePage(page) {
			this.currentPage = page;
		},
		addToCart(product) {
			const itemInCart = this.cart.find(item => item.id === product.id);
			if (itemInCart) {
				itemInCart.quantity += product.quantity;
			} else {
				this.cart.push({
					id: product.id,
					name: product.name,
					quantity: product.quantity,
				});
			}
			},
	},
	mounted() {
		// Fetch categories and products data from API or static file
		// Assign the data to this.categories and this.products
		// For simplicity, let's assume the data is already available
		
		// Example data
		this.categories = [
			{ id: 1, name: 'Category 1' },
			{ id: 2, name: 'Category 2' },
			{ id: 3, name: 'Category 3' },
		];
		
		this.products = [
			{
				id: 1,
				name: 'Product 1',
				description: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit.',
				price: 10.99,
				image: 'path/to/product1.jpg',
				quantity: 1,
			},
			{
				id: 2,
				name: 'Product 2',
				description: 'Nullam facilisis metus at aliquam pharetra.',
				price: 19.99,
				image: 'path/to/product2.jpg',
				quantity: 1,
			},
			// Add more products here
		];
	},
};
</script>

<style>
.sidebar {
	margin-top: 20px;
}

.search-bar {
	margin-bottom: 20px;
}

.products {
	margin-bottom: 20px;
}

.card {
	margin-bottom: 20px;
}

.card-img-top {
	height: 200px;
	object-fit: cover;
}

.card-actions {
	margin-top: 10px;
}

.pagination {
	margin-top: 20px;
}
</style>
