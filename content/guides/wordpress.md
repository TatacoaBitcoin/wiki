---
title: 'Wordpress'
metaTitle: 'Guías - Wordpress'
metaDescription: 'How-to guides'
---

# Cambiar nombre de dominio

    mysql> select * from wp_options where option_name in ('siteurl', 'home');
    mysql> update wp_options set option_value='https://www.tatacoabitcoin.com' where option_name in ('siteurl', 'home');
