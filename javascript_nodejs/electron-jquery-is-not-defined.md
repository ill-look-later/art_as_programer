# Electron: jQuery is not defined


A better an more generic solution IMO:
---

```javascript
<!-- Insert this line above script imports -->
<script>
  if (typeof module === 'object') {
    window.module = module; module = undefined;
  }
</script> 
<!-- normal script imports etc --> 
<script src="scripts/jquery.min.js"></script> 
<script src="scripts/vendor.js"></script> 
<!-- Insert this line after script imports --> 
<script>
  if (window.module) 
    module = window.module;
</script>
```
