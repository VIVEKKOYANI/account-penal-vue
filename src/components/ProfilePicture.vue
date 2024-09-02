<template>
  <img :src="fetchGravatar || image" alt="Profile Image" />
</template>

<script>
import { computed } from 'vue';
import CryptoJS from 'crypto-js';

export default {
  props: ["email", 'image'],
  emits: ['update:image'],
  setup(props, { emit }) {
    
    const fetchGravatar = computed(() => {
      if (props.email) {
        const hash = getMD5Hash(props.email);
        const gravatarImage = `https://www.gravatar.com/avatar/${hash}?d=identicon`;
        emit('update:image', gravatarImage); // Emit the new image URL to the parent
        return gravatarImage;
      } else {
        return null;
      }
    });

    const getMD5Hash = (email) => {
      return CryptoJS.MD5(email.trim().toLowerCase()).toString();
    };

    return {
      image: props.image,
      fetchGravatar
    };
  }
};
</script>