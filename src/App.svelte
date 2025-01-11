<script>
  import {Container, Row, Col, Form, Input, Button} from '@sveltestrap/sveltestrap'

  let proto = ''
  let id = ''
  let routePath = ''
  let verb = ''
  let fsPath = ''
  let mainHeader = ''
  let mainData = ''
  let hashOrKey = ''
  let extra = ''
  async function func(e){
    e.preventDefault()
    const ft = await fetch('http://localhost:13579/', {method: 'POST', headers: {'Content-Type': 'application.json'}, body: JSON.stringify({extra, proto, id, verb, path: routePath, data: fsPath, address: hashOrKey === 'key' ? true : false})})
    if(ft.headers.has('X-ID')){
      mainHeader = ft.headers.get('X-ID')
    }
    mainData = await ft.text()
  }
</script>

<Container fluid>
  <Row>
    <Col>
      <Form on:submit={func}>
        <Input type="text" placeholder="proto" bind:value={proto}></Input>
        <Input type="text" placeholder="id" bind:value={id}></Input>
        <Input type="text" placeholder="url route path" bind:value={routePath}></Input>
        <Input type="text" placeholder="verb" bind:value={verb}></Input>
        <Input type="text" placeholder="file system path" bind:value={fsPath}></Input>
        {#if proto === 'bt'}
          <Input type="text" placeholder="secret" bind:value={extra}></Input>
          {#each ['hash', 'key'] as value}
            <Input type="radio" bind:group={hashOrKey} {value} label={value.charAt(0).toUpperCase() + value.slice(1)} />
          {/each}
        {/if}
        <Button type="submit">submit</Button>
      </Form>
    </Col>
  </Row>
  {#if mainHeader}
    <Row>
      <Col>
        <p>{mainHeader}</p>
      </Col>
    </Row>
  {/if}
  {#if mainData}
    <Row>
      <Col>
        <p>{mainData}</p>
      </Col>
    </Row>
  {/if}
</Container>

<style>
  .logo {
    height: 6em;
    padding: 1.5em;
    will-change: filter;
    transition: filter 300ms;
  }
  .logo:hover {
    filter: drop-shadow(0 0 2em #646cffaa);
  }
  .logo.svelte:hover {
    filter: drop-shadow(0 0 2em #ff3e00aa);
  }
  .read-the-docs {
    color: #888;
  }
</style>
