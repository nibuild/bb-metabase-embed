<script>
  import { getContext } from "svelte"
  import { sign } from "jsonwebtoken-esm" 

  export let metabaseURL
  export let metabaseType
  export let metabaseKey
  export let metabasePayload
  export let timeout
  export let bordered
  export let titled
  export let theme
  export let ewidth
  export let eheight

  metabasePayload.exp = Math.round(Date.now() / 1000) + (timeout * 60)
  let jwtPayload = sign(metabasePayload, metabaseKey)
  let setBorder = "true"
  let setTitle = "true"
  let setTheme = ""

  if ( !bordered ) {
    setBorder = false
  }
  if ( !titled ) {
    setTitle = false
  }
  if ( theme && theme != "default" ) {
    if ( theme == "dark" )
      setTheme = "theme=night&"
    if ( theme == "transparent" )
      setTheme = "theme=transparent&"
  }

  $: iframeUrl = metabaseURL + "/embed/" + metabaseType + "/" + jwtPayload + "#" + setTheme + "bordered=" + setBorder + "&titled=" + setTitle

  const { styleable } = getContext("sdk")
  const component = getContext("component")
</script>

<iframe
  use:styleable={$component.styles}
  title="Metabase Embed"
  src={iframeUrl}
  frameborder="0"
  width={ewidth}
  height={eheight}
  allowtransparency
/>

