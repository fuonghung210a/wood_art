<script setup lang="ts">
import ShopLayout from '@/layouts/shop/ShopLayout.vue';
import { Head } from '@inertiajs/vue3';
import { type BreadcrumbItemType } from '@/types';
import { ref } from 'vue';

interface Product {
    id: number;
    name: string;
    slug: string;
    description: string;
    images: string[];
    variants: Array<{
        id: number;
        name: string;
        price: number;
        stock: number;
    }>;
    categories: Array<{
        id: number;
        name: string;
        slug: string;
    }>;
    reviews: Array<{
        id: number;
        rating: number;
        comment: string;
        user: {
            name: string;
        };
        created_at: string;
    }>;
    average_rating: number;
    reviews_count: number;
}

interface Props {
    product: Product;
}

const props = defineProps<Props>();

const selectedVariant = ref(props.product.variants[0]);
const selectedImage = ref(props.product.images[0]);
const quantity = ref(1);

const breadcrumbs: BreadcrumbItemType[] = [
    { title: 'Shop', href: '/shop' },
    ...props.product.categories.map(cat => ({
        title: cat.name,
        href: `/shop/categories/${cat.slug}`,
    })),
    { title: props.product.name, href: `/shop/products/${props.product.slug}` },
];
</script>

<template>
    <Head :title="`${product.name} - Wood Art`" />
    
    <ShopLayout :breadcrumbs="breadcrumbs">
        <div class="grid grid-cols-1 gap-8 lg:grid-cols-2">
            <!-- Product Images -->
            <div class="space-y-4">
                <!-- Main Image -->
                <div class="aspect-square overflow-hidden rounded-lg border bg-muted">
                    <img 
                        v-if="selectedImage"
                        :src="selectedImage" 
                        :alt="product.name"
                        class="h-full w-full object-cover"
                    />
                    <div v-else class="flex h-full w-full items-center justify-center">
                        <span class="text-muted-foreground">No image available</span>
                    </div>
                </div>
                
                <!-- Image Thumbnails -->
                <div v-if="product.images.length > 1" class="flex space-x-2">
                    <button
                        v-for="image in product.images"
                        :key="image"
                        @click="selectedImage = image"
                        class="h-20 w-20 overflow-hidden rounded border"
                        :class="{ 'ring-2 ring-primary': selectedImage === image }"
                    >
                        <img 
                            :src="image" 
                            :alt="product.name"
                            class="h-full w-full object-cover"
                        />
                    </button>
                </div>
            </div>
            
            <!-- Product Details -->
            <div class="space-y-6">
                <div>
                    <h1 class="text-3xl font-bold tracking-tight">{{ product.name }}</h1>
                    <p class="mt-2 text-muted-foreground">{{ product.description }}</p>
                </div>
                
                <!-- Rating -->
                <div class="flex items-center space-x-2">
                    <div class="flex">
                        <span 
                            v-for="star in 5" 
                            :key="star"
                            class="text-yellow-400"
                            :class="star <= product.average_rating ? 'fill-current' : 'opacity-30'"
                        >
                            ★
                        </span>
                    </div>
                    <span class="text-sm text-muted-foreground">
                        {{ product.average_rating }} ({{ product.reviews_count }} reviews)
                    </span>
                </div>
                
                <!-- Price -->
                <div class="text-3xl font-bold">
                    ${{ selectedVariant?.price || 0 }}
                </div>
                
                <!-- Categories -->
                <div>
                    <span class="text-sm font-medium text-muted-foreground">Categories: </span>
                    <span class="text-sm">
                        {{ product.categories.map(cat => cat.name).join(', ') }}
                    </span>
                </div>
                
                <!-- Variants -->
                <div v-if="product.variants.length > 1">
                    <label class="text-sm font-medium">Variant:</label>
                    <select 
                        v-model="selectedVariant"
                        class="mt-1 block w-full rounded-md border border-input bg-background px-3 py-2"
                    >
                        <option 
                            v-for="variant in product.variants" 
                            :key="variant.id"
                            :value="variant"
                        >
                            {{ variant.name }} - ${{ variant.price }}
                        </option>
                    </select>
                </div>
                
                <!-- Quantity -->
                <div>
                    <label class="text-sm font-medium">Quantity:</label>
                    <div class="mt-1 flex items-center space-x-2">
                        <button 
                            @click="quantity = Math.max(1, quantity - 1)"
                            class="flex h-8 w-8 items-center justify-center rounded border border-input bg-background hover:bg-accent"
                        >
                            -
                        </button>
                        <input 
                            v-model.number="quantity"
                            type="number"
                            min="1"
                            :max="selectedVariant?.stock || 0"
                            class="h-8 w-16 rounded border border-input bg-background px-2 text-center"
                        />
                        <button 
                            @click="quantity = Math.min(selectedVariant?.stock || 0, quantity + 1)"
                            class="flex h-8 w-8 items-center justify-center rounded border border-input bg-background hover:bg-accent"
                        >
                            +
                        </button>
                    </div>
                    <p class="mt-1 text-sm text-muted-foreground">
                        {{ selectedVariant?.stock || 0 }} in stock
                    </p>
                </div>
                
                <!-- Actions -->
                <div class="flex space-x-4">
                    <button 
                        class="flex-1 rounded bg-primary px-6 py-3 font-semibold text-primary-foreground hover:bg-primary/90 disabled:opacity-50"
                        :disabled="!selectedVariant || selectedVariant.stock === 0"
                    >
                        Add to Cart
                    </button>
                    <button class="rounded border border-input px-6 py-3 hover:bg-accent">
                        Add to Wishlist
                    </button>
                </div>
                
                <!-- Product Information -->
                <div class="border-t pt-6">
                    <h3 class="font-semibold mb-2">Product Information</h3>
                    <div class="space-y-2 text-sm">
                        <div class="flex justify-between">
                            <span class="text-muted-foreground">SKU:</span>
                            <span>{{ selectedVariant?.id || product.id }}</span>
                        </div>
                        <div class="flex justify-between">
                            <span class="text-muted-foreground">Material:</span>
                            <span>Premium Wood</span>
                        </div>
                        <div class="flex justify-between">
                            <span class="text-muted-foreground">Availability:</span>
                            <span :class="selectedVariant?.stock > 0 ? 'text-green-600' : 'text-destructive'">
                                {{ selectedVariant?.stock > 0 ? 'In Stock' : 'Out of Stock' }}
                            </span>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- Reviews Section -->
        <div v-if="product.reviews.length > 0" class="mt-16 border-t pt-16">
            <h2 class="text-2xl font-bold mb-6">Customer Reviews</h2>
            <div class="space-y-6">
                <div 
                    v-for="review in product.reviews" 
                    :key="review.id"
                    class="border-b pb-6 last:border-b-0"
                >
                    <div class="flex items-start justify-between">
                        <div>
                            <div class="flex items-center space-x-2 mb-2">
                                <div class="flex">
                                    <span 
                                        v-for="star in 5" 
                                        :key="star"
                                        class="text-yellow-400"
                                        :class="star <= review.rating ? 'fill-current' : 'opacity-30'"
                                    >
                                        ★
                                    </span>
                                </div>
                                <span class="font-medium">{{ review.user.name }}</span>
                            </div>
                            <p class="text-muted-foreground">{{ review.comment }}</p>
                        </div>
                        <span class="text-sm text-muted-foreground">
                            {{ new Date(review.created_at).toLocaleDateString() }}
                        </span>
                    </div>
                </div>
            </div>
        </div>
    </ShopLayout>
</template>
