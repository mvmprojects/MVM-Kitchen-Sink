# Angular

## Form Samples

### Basic form

Basic contact form with field validators (no angular materials and no custom validators).

#### Typescript

	import { Component, OnInit } from '@angular/core';
	import { FormBuilder, FormControl, FormGroup, Validators } from '@angular/forms';

	@Component({
	  selector: 'app-contact',
	  templateUrl: './contact.component.html',
	  styleUrls: ['./contact.component.scss']
	})
	export class ContactComponent implements OnInit {

	  formMain!: FormGroup;

	  labelName = 'Name';
	  labelEmail = 'Email';
	  labelMessage = 'Message'; 
	  requirementWarning = 'This field is required.';
	  emailWarning = 'Please enter a valid email address.';

	  constructor(private formBuilder: FormBuilder){ 
		this.formMain = this.formBuilder.group({
		  contactName: new FormControl('', [Validators.required]),
		  contactEmail: new FormControl('', [Validators.required, Validators.email]),  
		  contactMessage: new FormControl('', [Validators.required])
		});
	  }

	  ngOnInit(): void {
	  }

	  onSubmit(): void {
		console.log(this.formMain.value);
	  }

	}

#### HTML

	<h2>Contact Form</h2>
	<form [formGroup]="formMain" (ngSubmit)="onSubmit()">
		<p class="label-small">{{labelName}}</p>
		<div>
			<input type="text" formControlName="contactName" [placeholder]="labelName"/>
		</div>
		<div class="txt-error" *ngIf="formMain.controls['contactName'].invalid  && (formMain.controls['contactName'].dirty || formMain.controls['contactName'].touched)">
			<div class="label-small" *ngIf="formMain.controls['contactName'].errors!['required']">{{requirementWarning}}</div>
		</div>    
		<p class="label-small">{{labelEmail}}</p>
		<div>
			<input type="text" formControlName="contactEmail" [placeholder]="labelEmail"/>
		</div>
		<div class="txt-error" *ngIf="formMain.controls['contactEmail'].invalid  && (formMain.controls['contactEmail'].dirty || formMain.controls['contactEmail'].touched)">
			<div class="label-small" *ngIf="formMain.controls['contactEmail'].errors!['required']">{{requirementWarning}}</div>
		</div>        
		<div class="txt-error" *ngIf="formMain.controls['contactEmail'].invalid  && (formMain.controls['contactEmail'].dirty || formMain.controls['contactEmail'].touched)">
			<div class="label-small" *ngIf="formMain.controls['contactEmail'].errors!['email']">{{emailWarning}}</div>
		</div>            
		<p class="label-small">{{labelMessage}}</p>
		<div>
			<input type="text" formControlName="contactMessage" [placeholder]="labelMessage"/>
		</div>        
		<div class="txt-error" *ngIf="formMain.controls['contactMessage'].invalid  && (formMain.controls['contactMessage'].dirty || formMain.controls['contactMessage'].touched)">
			<div class="label-small" *ngIf="formMain.controls['contactMessage'].errors!['required']">{{requirementWarning}}</div>
		</div>        
		<div>
			<button type="submit" [disabled]="formMain.invalid" class="submit-button">Submit</button>
		</div>
	</form>
	
#### Style 

	form p {
		margin: 0.5em 0 0 0
	}

	form input {
		margin: 0 0 0 0.5em
	}
	
	.label-small {
		font-size: small;
	}

	.txt-error {
		color: maroon;
	}

	.submit-button {
		margin: 1em; padding: 0.5em;
	}	
	
### Creation Form 

What follows is a form for adding an object, using Angular Materials and custom validators. 

Not included: a fully implemented submit button.

NOTE: the angular materials in this example require MatInputModule to be imported either in app.module or in your own custom module used for grouping materials together.

	import { MatInputModule } from '@angular/material/input';
	
	(...)
	
	@NgModule({
    declarations: [],
    imports: [
        CommonModule
    ],
    exports: [
		(...)
        MatInputModule
    ]
	})

#### Typescript

	import { Component, OnInit } from '@angular/core';
	import { FormBuilder, FormControl, FormGroup, Validators } from '@angular/forms';

	@Component({
	  selector: 'app-add-phone',
	  templateUrl: './add-phone.component.html',
	  styleUrls: ['./add-phone.component.scss']
	})
	export class AddPhoneComponent implements OnInit {

	  formMain!: FormGroup;
	  labelBrand = 'Brand';
	  labelType = 'Type';
	  labelDescription = 'Description'; 
	  labelPrice = 'Price';
	  labelStock = 'Stock';
	  decimalRegex = '^\-?[0-9]+(?:\.[0-9]{1,2})?$';
	  positiveIntRegex = '^[0-9]+$';
	  requirementWarning = 'This field is required.';
	  valueWarning = 'Please enter a valid value.';

	  constructor(private formBuilder: FormBuilder){
		this.formMain = this.formBuilder.group({
		  phoneBrand: new FormControl('', [Validators.required]),
		  phoneType: new FormControl('', [Validators.required]),
		  phoneStock: new FormControl('', 
		  [
			Validators.required, 
			Validators.pattern(this.positiveIntRegex)
		  ]),
		  phonePrice: new FormControl('', 
		  [
			Validators.required, 
			Validators.min(0), 
			Validators.pattern(this.decimalRegex)
		  ]),
		  phoneDescription: new FormControl(''),      
		});
	  }

	  ngOnInit(): void {
	  }

	  submitForm(): void {
		
	  }

	}


#### Html

	<h2>Add Phone</h2>
	<form [formGroup]="formMain" style="display:flex; flex-direction: column;">

		<mat-form-field>
			<input matInput [placeholder]="labelBrand" formControlName="phoneBrand" required type="text">
		</mat-form-field>
		<div *ngIf="formMain.controls['phoneBrand'].invalid  && (formMain.controls['phoneBrand'].dirty || formMain.controls['phoneBrand'].touched)">
			<mat-error *ngIf="formMain.controls['phoneBrand'].errors!['required']">{{requirementWarning}}</mat-error>
		</div>    

		<mat-form-field>
			<input matInput [placeholder]="labelType" formControlName="phoneType" required type="text">
		</mat-form-field>    
		<div *ngIf="formMain.controls['phoneType'].invalid  && (formMain.controls['phoneType'].dirty || formMain.controls['phoneType'].touched)">
			<mat-error *ngIf="formMain.controls['phoneType'].errors!['required']">{{requirementWarning}}</mat-error>
		</div>        

		<mat-form-field>
			<input matInput [placeholder]="labelPrice" formControlName="phonePrice" required type="text">
		</mat-form-field>
		<div class="txt-error" *ngIf="formMain.controls['phonePrice'].invalid  && (formMain.controls['phonePrice'].dirty || formMain.controls['phonePrice'].touched)">
			<mat-error *ngIf="formMain.controls['phonePrice'].errors!['required']">{{requirementWarning}}</mat-error>
			<mat-error *ngIf="formMain.controls['phonePrice'].errors!['min']">{{valueWarning}}</mat-error>
			<mat-error *ngIf="formMain.controls['phonePrice'].errors!['pattern']">{{valueWarning}}</mat-error>
		</div>            

		<mat-form-field>
			<input matInput [placeholder]="labelStock" formControlName="phoneStock" required type="text">
		</mat-form-field>    
		<div *ngIf="formMain.controls['phoneStock'].invalid  && (formMain.controls['phoneStock'].dirty || formMain.controls['phoneStock'].touched)">
			<mat-error *ngIf="formMain.controls['phoneStock'].errors!['required']">{{requirementWarning}}</mat-error>
			<mat-error *ngIf="formMain.controls['phoneStock'].errors!['pattern']">{{valueWarning}}</mat-error>
		</div>            

		<mat-form-field>
			<input matInput [placeholder]="labelDescription" formControlName="phoneDescription" type="text">
		</mat-form-field>
		<div>
			<button mat-raised-button (click)="submitForm" [disabled]="formMain.invalid" class="submit-button">
				Submit
			</button>
		</div>    
	</form>

#### Style

	mat-form-field {
		width: 25em;
	}
	
	.txt-error {
		color: maroon;
	}

	.submit-button {
		margin: 1em; padding: 0.5em;
	}		