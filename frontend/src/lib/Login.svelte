<script lang="ts">
    import "../app.css";
    import svelteLogo from '../assets/svelte.svg'
    import viteLogo from '/vite.svg'
    import * as Card from "./components/ui/card"
    import { Input } from "./components/ui/input"
    import { Button } from "./components/ui/button"
    import { Label } from "./components/ui/label"
  
    let current: string = "signup";
    let password_valid: boolean = false;
    let username_valid: boolean = false;
    let username: string = "";
    let password: string = "";
    export let token: string | null = null;
  
    function validatePassword(event: Event): void {
      const target = event.target as HTMLInputElement;
      const regex = /^[A-Za-z!@#$%^&*(),.?":{}|<>[\]~`+=_-]+$/;
      password_valid = regex.test(target.value);
    }
  
    function validateUsername(event: Event): void {
      const target = event.target as HTMLInputElement;
      const regex = /^[A-Za-z!@#$%^&*(),.?":{}|<>[\]~`+=_-]+$/;
      username_valid = regex.test(target.value);
    }
  
  async function handleSubmit() {
    const url = current === "signup" ? "/signup" : "/login";
    const method = "POST";
    const body = JSON.stringify({ username, password });
    console.log(body);
    const response = await fetch(`http://127.0.0.1:5000${url}`, {
      method,
      headers: {
        "Content-Type": "application/json",
        'Access-Control-Allow-Methods': 'GET, POST, PUT, DELETE, PATCH, OPTIONS',
                    'Access-Control-Allow-Origin': '*', // Change to allow only specific domains
                    'Access-Control-Allow-Headers': '*' 
      },
      body
    });
    
    const data = await response.json();
    
    if (response.ok) {
      // Save JWT token on success
      if (data.token) {
        token = data.token;
        localStorage.setItem("token", token as string); // Store JWT in localStorage
        alert(data.message);
      }
    } else {
      alert(data.message);
    }
  }
  </script>
  
    <div class="h-screen flex items-center justify-center">
      <Card.Root class="w-[350px]">
        <Card.Header>
          <Card.Title>HackByte</Card.Title>
          <Card.Description> { current == "signup" ? "Sign Up" : "Login" } </Card.Description>
          <div class="justify-around w-max">
            <Button on:click={() => {current = "signup"}}> Sign-Up </Button>
            <Button on:click={() => {current = "login"}}> Login </Button>
          </div>
        </Card.Header>
        <Card.Content>
          <div class="flex w-full max-w-sm flex-col gap-1.5">
            <Label for="username-2">Username</Label>
            <Input type="username" id="username-2" placeholder="Username" on:input={validateUsername} bind:value={username}/>
            { #if !username_valid}
              <p class="text-muted-foreground text-sm text-red-500 opacity-70">Username should not be empty. Should only contain A-Z and _</p>  
            {/if}
          </div>
          <br>
          <div class="flex w-full max-w-sm flex-col gap-1.5">
            <Label for="password-2">Password</Label>
            <Input type="password" id="password-2" placeholder="Password" on:input={validatePassword} bind:value={password}/>
  
            { #if !password_valid}
              <p class="text-muted-foreground text-sm text-red-500 opacity-70">Password should not be empty. Should only contain A-Z and _</p>  
            {/if}
          </div>
        </Card.Content>
        <Card.Footer class="justify-around">
          <Button on:click={handleSubmit}>Submit</Button>
        </Card.Footer>
      </Card.Root>
    </div>
  