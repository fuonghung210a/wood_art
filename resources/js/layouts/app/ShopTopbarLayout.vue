<script setup lang="ts">
import { Link } from '@inertiajs/vue3';
import { ChevronRight, SlidersHorizontal } from 'lucide-vue-next';
import type { BreadcrumbItemType } from '@/types';
import { ref } from 'vue';

interface Props {
    breadcrumbs?: BreadcrumbItemType[];
    showCategories?: boolean;
    showFilters?: boolean;
}

withDefaults(defineProps<Props>(), {
    breadcrumbs: () => [],
    showCategories: true,
    showFilters: true,
});

const filtersOpen = ref(false);

// Sample categories - these should come from your backend
const categories = [
    { name: 'Furniture', slug: 'furniture', href: '/shop/categories/furniture' },
    { name: 'Decor', slug: 'decor', href: '/shop/categories/decor' },
    { name: 'Kitchen', slug: 'kitchen', href: '/shop/categories/kitchen' },
    { name: 'Outdoor', slug: 'outdoor', href: '/shop/categories/outdoor' },
    { name: 'Storage', slug: 'storage', href: '/shop/categories/storage' },
    { name: 'Lighting', slug: 'lighting', href: '/shop/categories/lighting' },
];
</script>

<template>
    <div class="border-b bg-muted/50">
        <div class="container mx-auto px-4">
            <!-- Categories Bar -->
            <div v-if="showCategories" class="border-b py-3">
                <div class="flex items-center justify-between">
                    <nav class="flex space-x-6 overflow-x-auto">
                        <Link
                            v-for="category in categories"
                            :key="category.slug"
                            :href="category.href"
                            class="whitespace-nowrap text-sm font-medium text-muted-foreground transition-colors hover:text-foreground"
                            :class="{ 'text-primary': $page.url.includes(category.slug) }"
                        >
                            {{ category.name }}
                        </Link>
                    </nav>
                    
                    <!-- Filter Toggle -->
                    <button 
                        v-if="showFilters"
                        @click="filtersOpen = !filtersOpen"
                        class="flex items-center space-x-2 rounded-md border border-input bg-background px-3 py-1.5 text-sm hover:bg-accent"
                    >
                        <SlidersHorizontal class="h-4 w-4" />
                        <span>Filters</span>
                    </button>
                </div>
            </div>

            <!-- Breadcrumbs -->
            <div v-if="breadcrumbs.length > 0" class="py-3">
                <nav class="flex" aria-label="Breadcrumb">
                    <ol class="flex items-center space-x-2">
                        <li>
                            <Link href="/" class="text-sm text-muted-foreground hover:text-foreground">
                                Home
                            </Link>
                        </li>
                        <ChevronRight class="h-4 w-4 text-muted-foreground" />
                        <li v-for="(item, index) in breadcrumbs" :key="index" class="flex items-center space-x-2">
                            <Link 
                                v-if="item.href && index < breadcrumbs.length - 1"
                                :href="item.href"
                                class="text-sm text-muted-foreground hover:text-foreground"
                            >
                                {{ item.title }}
                            </Link>
                            <span 
                                v-else
                                class="text-sm font-medium text-foreground"
                            >
                                {{ item.title }}
                            </span>
                            <ChevronRight 
                                v-if="index < breadcrumbs.length - 1"
                                class="h-4 w-4 text-muted-foreground" 
                            />
                        </li>
                    </ol>
                </nav>
            </div>

            <!-- Filters Panel (Collapsible) -->
            <div 
                v-if="showFilters && filtersOpen"
                class="border-t py-4"
            >
                <div class="grid grid-cols-1 gap-4 md:grid-cols-4">
                    <!-- Price Range -->
                    <div>
                        <label class="text-sm font-medium mb-2 block">Price Range</label>
                        <div class="flex items-center space-x-2">
                            <input 
                                type="number" 
                                placeholder="Min"
                                class="w-full rounded border border-input bg-background px-2 py-1 text-sm"
                            />
                            <span class="text-muted-foreground">-</span>
                            <input 
                                type="number" 
                                placeholder="Max"
                                class="w-full rounded border border-input bg-background px-2 py-1 text-sm"
                            />
                        </div>
                    </div>

                    <!-- Sort By -->
                    <div>
                        <label class="text-sm font-medium mb-2 block">Sort By</label>
                        <select class="w-full rounded border border-input bg-background px-2 py-1 text-sm">
                            <option value="newest">Newest</option>
                            <option value="price-low">Price: Low to High</option>
                            <option value="price-high">Price: High to Low</option>
                            <option value="popular">Most Popular</option>
                            <option value="rating">Highest Rated</option>
                        </select>
                    </div>

                    <!-- Availability -->
                    <div>
                        <label class="text-sm font-medium mb-2 block">Availability</label>
                        <div class="space-y-1">
                            <label class="flex items-center space-x-2">
                                <input type="checkbox" class="rounded border-input" />
                                <span class="text-sm">In Stock</span>
                            </label>
                            <label class="flex items-center space-x-2">
                                <input type="checkbox" class="rounded border-input" />
                                <span class="text-sm">On Sale</span>
                            </label>
                        </div>
                    </div>

                    <!-- Rating -->
                    <div>
                        <label class="text-sm font-medium mb-2 block">Rating</label>
                        <div class="space-y-1">
                            <label class="flex items-center space-x-2">
                                <input type="checkbox" class="rounded border-input" />
                                <span class="text-sm">4+ Stars</span>
                            </label>
                            <label class="flex items-center space-x-2">
                                <input type="checkbox" class="rounded border-input" />
                                <span class="text-sm">3+ Stars</span>
                            </label>
                        </div>
                    </div>
                </div>

                <div class="mt-4 flex justify-end space-x-2">
                    <button class="rounded border border-input bg-background px-4 py-2 text-sm hover:bg-accent">
                        Clear All
                    </button>
                    <button class="rounded bg-primary px-4 py-2 text-sm text-primary-foreground hover:bg-primary/90">
                        Apply Filters
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>
