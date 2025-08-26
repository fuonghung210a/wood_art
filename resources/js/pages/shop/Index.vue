<script setup lang="ts">
import ShopLayout from '@/layouts/shop/ShopLayout.vue';
import { Head } from '@inertiajs/vue3';
import { type BreadcrumbItemType } from '@/types';

interface Product {
    id: number;
    name: string;
    slug: string;
    description: string;
    image?: string;
    price: number;
    rating: number;
    reviews_count: number;
    in_stock: boolean;
    on_sale?: boolean;
    original_price?: number;
}

interface Props {
    products: Product[];
    total: number;
    per_page: number;
    current_page: number;
}

defineProps<Props>();

const breadcrumbs: BreadcrumbItemType[] = [
    {
        title: 'Shop',
        href: '/shop',
    },
];
</script>

<template>
    <Head title="Shop - Wood Art" />
    <ShopLayout :breadcrumbs="breadcrumbs">
        <div class="space-y-6">
            <!-- Page Header -->
            <div class="flex items-center justify-between">
                <div>
                    <h1 class="text-3xl font-bold tracking-tight">All Products</h1>
                    <p class="mt-2 text-muted-foreground">
                        Discover our handcrafted wooden products
                    </p>
                </div>
                <div class="text-sm text-muted-foreground">
                    {{ total }} products found
                </div>
            </div>

            <!-- Products Grid -->
            <div class="grid grid-cols-1 gap-6 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4">
                <div
                    v-for="product in products"
                    :key="product.id"
                    class="group relative overflow-hidden rounded-lg border bg-card transition-shadow hover:shadow-lg"
                >
                    <!-- Product Image -->
                    <div class="aspect-square overflow-hidden bg-muted">
                        <img
                            v-if="product.image"
                            :src="product.image"
                            :alt="product.name"
                            class="h-full w-full object-cover transition-transform group-hover:scale-105"
                        />
                        <div v-else class="flex h-full w-full items-center justify-center">
                            <span class="text-muted-foreground">No image</span>
                        </div>

                        <!-- Sale Badge -->
                        <div
                            v-if="product.on_sale"
                            class="absolute left-2 top-2 rounded bg-destructive px-2 py-1 text-xs font-medium text-destructive-foreground"
                        >
                            Sale
                        </div>

                        <!-- Stock Status -->
                        <div
                            v-if="!product.in_stock"
                            class="absolute inset-0 flex items-center justify-center bg-background/80"
                        >
                            <span class="rounded bg-muted px-3 py-1 text-sm font-medium">
                                Out of Stock
                            </span>
                        </div>
                    </div>

                    <!-- Product Info -->
                    <div class="p-4">
                        <h3 class="font-semibold text-card-foreground line-clamp-2">
                            {{ product.name }}
                        </h3>
                        <p class="mt-1 text-sm text-muted-foreground line-clamp-2">
                            {{ product.description }}
                        </p>

                        <!-- Rating -->
                        <div class="mt-2 flex items-center space-x-1">
                            <div class="flex">
                                <span
                                    v-for="star in 5"
                                    :key="star"
                                    class="text-yellow-400"
                                    :class="star <= product.rating ? 'fill-current' : 'opacity-30'"
                                >
                                    ★
                                </span>
                            </div>
                            <span class="text-xs text-muted-foreground">
                                ({{ product.reviews_count }})
                            </span>
                        </div>

                        <!-- Price -->
                        <div class="mt-3 flex items-center justify-between">
                            <div class="space-x-2">
                                <span class="font-bold text-lg">
                                    ${{ product.price }}
                                </span>
                                <span
                                    v-if="product.on_sale && product.original_price"
                                    class="text-sm text-muted-foreground line-through"
                                >
                                    ${{ product.original_price }}
                                </span>
                            </div>
                            <button
                                :disabled="!product.in_stock"
                                class="rounded bg-primary px-3 py-1.5 text-sm font-medium text-primary-foreground transition-colors hover:bg-primary/90 disabled:opacity-50 disabled:cursor-not-allowed"
                            >
                                {{ product.in_stock ? 'Add to Cart' : 'Out of Stock' }}
                            </button>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Empty State -->
            <div
                v-if="products.length === 0"
                class="flex flex-col items-center justify-center py-16 text-center"
            >
                <div class="h-24 w-24 rounded-full bg-muted flex items-center justify-center mb-4">
                    <span class="text-4xl">🪵</span>
                </div>
                <h3 class="text-lg font-semibold mb-2">No products found</h3>
                <p class="text-muted-foreground">
                    Try adjusting your search or filter criteria.
                </p>
            </div>

            <!-- Pagination -->
            <div
                v-if="products.length > 0"
                class="flex items-center justify-center space-x-2 pt-6"
            >
                <button
                    :disabled="current_page === 1"
                    class="rounded border border-input bg-background px-3 py-1.5 text-sm hover:bg-accent disabled:opacity-50"
                >
                    Previous
                </button>
                <span class="text-sm text-muted-foreground">
                    Page {{ current_page }} of {{ Math.ceil(total / per_page) }}
                </span>
                <button
                    :disabled="current_page >= Math.ceil(total / per_page)"
                    class="rounded border border-input bg-background px-3 py-1.5 text-sm hover:bg-accent disabled:opacity-50"
                >
                    Next
                </button>
            </div>
        </div>
    </ShopLayout>
</template>
