<tk-switch-toggle (toggleState)="setToggle($event)"></tk-switch-toggle>
---------------------------------
<div class="toggle-container" 
    (click)="toggle()"
    [class.bg-white]="!isOn"
    [class.bg-black]="isOn"
>
  <div class="toggle-value" [class.is-off]="!isOn"></div>
</div>
------------------------------------------------
.toggle-container {
    position: relative;
    width: 64px;
    height: 32px;
    border-radius: 16px;
    cursor: pointer;
    border: 1px solid var(--tk-color-primary-black);

    .toggle-value {
      position: absolute;
      background-color: var(--tk-color-primary-white);
      border: 1px solid var(--tk-color-primary-black);
      height: 32px;
      width: 32px;
      border-radius: 50%;
      top: 50%;
      bottom: 50%;
      transform: translateY(-50%);
      right: -1%;
      transition: all 0.3s;
  
      &.is-off {
        right: 54%;
      }
    }
}

.bg-black {
    background-color: var(--tk-color-primary-black);
}
----------------------------
import { Component, EventEmitter, Input, Output } from '@angular/core';

@Component({
  selector: 'tk-switch-toggle',
  templateUrl: './tk-switch-toggle.component.html',
  styleUrls: ['./tk-switch-toggle.component.scss'],
})
export class TkSwitchToggle {
  @Input() public isOn = false;
  @Output() public toggleState: EventEmitter<boolean> = new EventEmitter();

  public toggle() {
    this.isOn = !this.isOn;
    this.toggleState.emit(this.isOn);
  }
}
----------------------------
