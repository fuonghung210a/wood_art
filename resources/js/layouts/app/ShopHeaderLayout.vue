<script setup lang="ts">
import { Link, usePage } from '@inertiajs/vue3';
import { ShoppingCart, User, Search, Menu, Heart } from 'lucide-vue-next';
import { ref, computed } from 'vue';

const page = usePage();
const user = computed(() => page.props.auth?.user);
const cartItemsCount = ref(0); // This should come from cart state
const mobileMenuOpen = ref(false);
</script>

<template>
    <header class="sticky top-0 z-50 border-b bg-background/95 backdrop-blur supports-[backdrop-filter]:bg-background/60">
        <div class="container mx-auto px-4">
            <div class="flex h-16 items-center justify-between">
                <!-- Logo -->
                <div class="flex items-center space-x-4">
                    <button
                        @click="mobileMenuOpen = !mobileMenuOpen"
                        class="lg:hidden"
                    >
                        <Menu class="h-6 w-6" />
                    </button>

                    <Link href="/" class="flex items-center space-x-2">
                        <div class="h-8 w-8 rounded bg-primary text-primary-foreground flex items-center justify-center font-bold">
                            W
                        </div>
                        <span class="hidden font-bold sm:inline-block">Wood Art</span>
                    </Link>
                </div>

                <!-- Desktop Navigation -->
                <nav class="hidden lg:flex lg:space-x-8">
                    <Link
                        href="/shop"
                        class="text-sm font-medium transition-colors hover:text-primary"
                        :class="{ 'text-primary': $page.url.startsWith('/shop') }"
                    >
                        Shop
                    </Link>
                    <Link
                        href="/shop/categories"
                        class="text-sm font-medium transition-colors hover:text-primary"
                    >
                        Categories
                    </Link>
                    <Link
                        href="/about"
                        class="text-sm font-medium transition-colors hover:text-primary"
                    >
                        About
                    </Link>
                    <Link
                        href="/contact"
                        class="text-sm font-medium transition-colors hover:text-primary"
                    >
                        Contact
                    </Link>
                </nav>

                <!-- Search Bar (Desktop) -->
                <div class="hidden md:flex flex-1 max-w-md mx-8">
                    <div class="relative w-full">
                        <Search class="absolute left-3 top-1/2 h-4 w-4 -translate-y-1/2 text-muted-foreground" />
                        <input
                            type="search"
                            placeholder="Search products..."
                            class="w-full rounded-md border border-input bg-background pl-10 pr-4 py-2 text-sm focus:border-primary focus:outline-none focus:ring-1 focus:ring-primary"
                        />
                    </div>
                </div>

                <!-- Right Side Actions -->
                <div class="flex items-center space-x-4">
                    <!-- Search (Mobile) -->
                    <button class="md:hidden">
                        <Search class="h-5 w-5" />
                    </button>

                    <!-- Wishlist -->
                    <Link
                        href="/wishlist"
                        class="relative hover:text-primary transition-colors"
                        title="Wishlist"
                    >
                        <Heart class="h-5 w-5" />
                    </Link>

                    <!-- Shopping Cart -->
                    <Link
                        href="/cart"
                        class="relative hover:text-primary transition-colors"
                        title="Shopping Cart"
                    >
                        <ShoppingCart class="h-5 w-5" />
                        <span
                            v-if="cartItemsCount > 0"
                            class="absolute -right-2 -top-2 flex h-4 w-4 items-center justify-center rounded-full bg-primary text-xs text-primary-foreground"
                        >
                            {{ cartItemsCount }}
                        </span>
                    </Link>

                    <!-- User Menu -->
                    <div class="relative">
                        <button
                            v-if="user"
                            class="flex items-center space-x-2 hover:text-primary transition-colors"
                        >
                            <User class="h-5 w-5" />
                            <span class="hidden sm:inline text-sm">{{ user.name }}</span>
                        </button>
                        <div v-else class="flex items-center space-x-2">
                            <Link
                                href="/login"
                                class="text-sm font-medium hover:text-primary transition-colors"
                            >
                                Login
                            </Link>
                            <Link
                                href="/register"
                                class="rounded bg-primary px-3 py-1.5 text-sm font-medium text-primary-foreground hover:bg-primary/90 transition-colors"
                            >
                                Sign Up
                            </Link>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Mobile Navigation Menu -->
            <div
                v-if="mobileMenuOpen"
                class="lg:hidden border-t py-4"
            >
                <nav class="flex flex-col space-y-4">
                    <Link
                        href="/shop"
                        class="text-sm font-medium transition-colors hover:text-primary"
                        @click="mobileMenuOpen = false"
                    >
                        Shop
                    </Link>
                    <Link
                        href="/shop/categories"
                        class="text-sm font-medium transition-colors hover:text-primary"
                        @click="mobileMenuOpen = false"
                    >
                        Categories
                    </Link>
                    <Link
                        href="/about"
                        class="text-sm font-medium transition-colors hover:text-primary"
                        @click="mobileMenuOpen = false"
                    >
                        About
                    </Link>
                    <Link
                        href="/contact"
                        class="text-sm font-medium transition-colors hover:text-primary"
                        @click="mobileMenuOpen = false"
                    >
                        Contact
                    </Link>
                </nav>

                <!-- Mobile Search -->
                <div class="mt-4 pt-4 border-t">
                    <div class="relative">
                        <Search class="absolute left-3 top-1/2 h-4 w-4 -translate-y-1/2 text-muted-foreground" />
                        <input
                            type="search"
                            placeholder="Search products..."
                            class="w-full rounded-md border border-input bg-background pl-10 pr-4 py-2 text-sm focus:border-primary focus:outline-none focus:ring-1 focus:ring-primary"
                        />
                    </div>
                </div>
            </div>
        </div>
    </header>
</template>


