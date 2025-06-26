# FinancasWeb processos da criação do projeto

npm i -g @angular/cli@20

ng version

ng new financasWeb

cd financasWeb

npm i --save bootstrap

code .

ng s -o


=============================================

            "styles": [
              "src/styles.css",
              "node_modules/bootstrap/dist/css/bootstrap.min.css"
            ],
            "scripts": [
              "node_modules/bootstrap/dist/js/bootstrap.bundle.min.js"
            ]

=============================================

<router-outlet/> 

=============================================

ng g c pages/criar-usuario --skip-tests

ng g c pages/autenticar-usuario --skip-tests

=============================================


import { Routes } from '@angular/router';
import { AutenticarUsuario } from './pages/autenticar-usuario/autenticar-usuario';
import { CriarUsuario } from './pages/criar-usuario/criar-usuario';

export const routes: Routes = [
    {
        path: 'autenticar', component: AutenticarUsuario
    },
    {
        path: 'criar-usuario', component: CriarUsuario
    },
    {
        path: '', pathMatch: 'full', redirectTo: '/autenticar'
    }
];

=============================================
fiz o html do componente authenticar:

"ChatGPT, pode criar um HTML estilizado com Bootstrap que tenha uma tela de login centralizada com logo, título, e campos de e-mail e senha, igual ao exemplo abaixo?"

=============================================
fiz o formulario par receber os dados da api java:

import { CommonModule } from '@angular/common';
import { Component } from '@angular/core';
import { FormControl, FormGroup, FormsModule, ReactiveFormsModule } from '@angular/forms';

@Component({
  selector: 'app-autenticar-usuario',
  imports: [
    CommonModule,
    FormsModule,
    ReactiveFormsModule
  ],
  templateUrl: './autenticar-usuario.html',
  styleUrl: './autenticar-usuario.css'
})
export class AutenticarUsuario {

  //Estrutura de formulário
  form = new FormGroup({
    email : new FormControl(''),
    senha : new FormControl('')
  });

  //Função para capturar o SUBMIT do formulário
  onSubmit() {
    
  }

}

=============================================
no html do authenticar:

formControlName="senha"   linha 17

formControlName="email"   linha 28

=============================================
na api , no spring montei o cors:

   package br.com.cotiinformatica.configurations;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
@EnableWebMvc
public class CorsConfiguration implements WebMvcConfigurer {

	@Override
	public void addCorsMappings(CorsRegistry registry) {
		
		registry.addMapping("/**")
			.allowedOrigins("http://localhost:4200")
			.allowedMethods("POST", "PUT", "DELETE", "GET")
			.allowedHeaders("*");
	}
}

=============================================
adicionei no app.config.ts o

provideHttpClient()

=============================================
depois atualizei o ts do auth usuario

import { CommonModule } from '@angular/common';
import { HttpClient } from '@angular/common/http';
import { Component, inject } from '@angular/core';
import { FormControl, FormGroup, FormsModule, ReactiveFormsModule } from '@angular/forms';

@Component({
  selector: 'app-autenticar-usuario',
  imports: [
    CommonModule,
    FormsModule,
    ReactiveFormsModule
  ],
  templateUrl: './autenticar-usuario.html',
  styleUrl: './autenticar-usuario.css'
})
export class AutenticarUsuario {

  //Injeção de dependência
  http = inject(HttpClient);

  //Estrutura de formulário
  form = new FormGroup({
    email : new FormControl(''),
    senha : new FormControl('')
  });

  //Função para capturar o SUBMIT do formulário
  onSubmit() {
    this.http.post('http://localhost:8082/api/v1/usuario/autenticar', this.form.value)
      .subscribe((response) => {
        console.log(response);
      })
  }

}
