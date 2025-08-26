<script setup lang="ts">
import ShopHeaderLayout from '@/layouts/app/ShopHeaderLayout.vue';
import ShopTopbarLayout from '@/layouts/app/ShopTopbarLayout.vue';
import type { BreadcrumbItemType } from '@/types';

interface Props {
    breadcrumbs?: BreadcrumbItemType[];
    showSidebar?: boolean;
    showTopbar?: boolean;
}

withDefaults(defineProps<Props>(), {
    breadcrumbs: () => [],
    showSidebar: true,
    showTopbar: true,
});
</script>

<template>
    <div class="min-h-screen bg-background">
        <!-- Shop Header (Navigation, Logo, Cart, User menu) -->
        <ShopHeaderLayout />
        
        <!-- Shop Topbar (Categories, Search, Filters) -->
        <ShopTopbarLayout v-if="showTopbar" :breadcrumbs="breadcrumbs" />
        
        <!-- Main Content Area -->
        <main class="container mx-auto px-4 py-6">
            <slot />
        </main>
        
        <!-- Footer -->
        <footer class="border-t bg-muted/50 py-8">
            <div class="container mx-auto px-4">
                <div class="grid grid-cols-1 gap-8 md:grid-cols-4">
                    <div>
                        <h3 class="mb-4 font-semibold">Wood Art Shop</h3>
                        <p class="text-sm text-muted-foreground">
                            Premium handcrafted wooden products for your home.
                        </p>
                    </div>
                    <div>
                        <h4 class="mb-4 font-medium">Quick Links</h4>
                        <ul class="space-y-2 text-sm">
                            <li><a href="/shop" class="text-muted-foreground hover:text-foreground">Shop</a></li>
                            <li><a href="/shop/categories" class="text-muted-foreground hover:text-foreground">Categories</a></li>
                            <li><a href="/about" class="text-muted-foreground hover:text-foreground">About</a></li>
                            <li><a href="/contact" class="text-muted-foreground hover:text-foreground">Contact</a></li>
                        </ul>
                    </div>
                    <div>
                        <h4 class="mb-4 font-medium">Customer Service</h4>
                        <ul class="space-y-2 text-sm">
                            <li><a href="/help" class="text-muted-foreground hover:text-foreground">Help Center</a></li>
                            <li><a href="/shipping" class="text-muted-foreground hover:text-foreground">Shipping Info</a></li>
                            <li><a href="/returns" class="text-muted-foreground hover:text-foreground">Returns</a></li>
                            <li><a href="/faq" class="text-muted-foreground hover:text-foreground">FAQ</a></li>
                        </ul>
                    </div>
                    <div>
                        <h4 class="mb-4 font-medium">Connect</h4>
                        <ul class="space-y-2 text-sm">
                            <li><a href="#" class="text-muted-foreground hover:text-foreground">Facebook</a></li>
                            <li><a href="#" class="text-muted-foreground hover:text-foreground">Instagram</a></li>
                            <li><a href="#" class="text-muted-foreground hover:text-foreground">Twitter</a></li>
                            <li><a href="#" class="text-muted-foreground hover:text-foreground">Pinterest</a></li>
                        </ul>
                    </div>
                </div>
                <div class="mt-8 border-t pt-8 text-center text-sm text-muted-foreground">
                    <p>&copy; 2025 Wood Art Shop. All rights reserved.</p>
                </div>
            </div>
        </footer>
    </div>
</template>
